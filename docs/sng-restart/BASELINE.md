# SNG v1–v4 restart baseline

Status: initial evidence inventory; no converter or player code changes.
Baseline: `main` at `e84042fa39916617db0357961b0992bbca3cf9c4`.
Working branch: `work/sng-v1-v4-restart`.

## Project direction

Glosa must ultimately support Ballade SNG versions 1.00 through 4.00.

The two implementation goals are separate:

- **SNG-to-SMF conversion:** independently analyze SNG2GS / SNG2S and reproduce its conversion behavior.
- **Direct SNG playback:** independently analyze MIMPI V4.27 and reproduce its direct SNG playback behavior. This path must not depend on converting SNG to SMF first.

SNG_DATA v1–v4 is the test corpus. JMPlayer is reference material only: its SNG implementation is incomplete and has no v1 support, so it is not a specification or an implementation baseline. Previous Ghidra exports and function labels are untrusted until independently checked against the original executable bytes and data flow.

## Primary references

- SNG2GS / SNG2S Ver. 3.3 executable and manual in the private research repository.
- MIMPI V4.27 executable and manual in the private research repository.
- SNG_DATA v1, v2, v3, and v4, with paired SNG2GS MIDI outputs where available.
- Existing Glosa implementation and tests in this repository are historical implementation evidence, not proof of format correctness.

Do not copy private SNG/MID fixtures, executables, or reverse-engineering dumps into this public repository.

## Confirmed manual facts

### SNG2S Ver. 3.3

The SNG2S manual identifies the utility as a converter from Music-kun / Music Rou / Ballade SNG files to Standard MIDI Files.

- Supported Ballade identifiers: 1.00 (Ballade / Music-kun), 2.00 (Ballade2 / Music Rou), 3.00 (Music Rou 2), 4.00 (Ballade3).
- MIDI time division: fixed at 48 ticks per quarter note.
- Default output: SMF Format 1.
- Default SysEx placement: `/X2`, a dedicated track immediately after the conductor track.
- `/X1` places SysEx in the conductor track; `/XD` disables SysEx conversion.
- Default part order for versions other than Ballade3: 10 down to 1.
- Default part order for Ballade3: 10, 16 down to 11, then 9 down to 1.
- `/F` selects ascending part order.
- `/N` omits parts without note data. Version 3.3 specifically fixed conversion of Ballade3 tracks 11 and above when `/N` is used.

The executable contains the identifying text `SNG2S Ver 3.3`. This confirms the version string embedded in the inspected executable; it does not by itself prove runtime behavior.

### MIMPI V4.27

The MIMPI V4.27 manual lists .SNG playback for Music-kun, Music Rou, Music Rou 2, Ballade, Ballade2, and Ballade3. It separately lists Standard MIDI File playback.

This establishes that MIMPI's SNG capability is a direct input format, not merely an SMF playback feature. The manual's supported-format list does not disclose the internal binary parser or prove every edge case of every SNG version.

## Initial binary and sample observations

The inspected MIMPI executable is an MZ DOS executable. Its current private-repository copy reports:

- File length: 136,947 bytes.
- MZ image length from the MZ page fields: 125,247 bytes.
- Trailing appended region: 11,700 bytes.
- MZ relocation count: 627.

An earlier handoff records 626 relocations for a MIMPI executable. Do not silently reconcile these values: first verify that the earlier Ghidra input and the current private-repository executable are byte-identical. Function addresses and labels from an export must not be reused until this identity check is complete.

Representative paired SNG2S reference files were inspected for all four versions. Their reference SMFs are Format 1 with division 48 and have 12 tracks for v1–v3 and 18 tracks for v4. These counts are consistent with a conductor track, a dedicated SysEx track, and 10 or 16 part tracks. The v1 reference includes tempo events at ticks 0, 8, and 192; its SysEx track contains four events. These are observations for the inspected samples, not yet universal rules for every file.

## Existing Glosa implementation: gaps to re-verify

The current `src/Glosa.Core/Sng/SngConverter.cs` explicitly states that its layout and tempo conversion are based on JMPlayer's `sngmidi.cpp`.

The current converter implementation:

- Iterates parts in ascending source order.
- Writes a conductor track and one track per parsed part.
- Does not currently construct the SNG2S default dedicated SysEx track.
- Has version-specific code paths, including a v1 path, but those paths must be treated as hypotheses until checked against SNG2S source behavior and paired references.

The current tests do not establish complete conversion equivalence:

- The general fixture test checks that output is readable and has at least two tracks.
- The v1 test checks note event counts and the presence of selected tempo byte sequences; it explicitly does not compare every event against the reference MIDI.
- Synthetic tests cover selected parser and Tie cases, not all SNG2S options or all version-specific event types.

Therefore, a successful conversion or a passing current test suite is not sufficient evidence of SNG2S compatibility.

## Evidence policy

Every finding must be labelled as one of:

1. **Confirmed:** directly supported by the manual, raw executable bytes, raw SNG/MID data, or independently verified source behavior.
2. **Hypothesis:** a proposed interpretation with explicit supporting evidence and a falsifiable check.
3. **Unknown:** not yet established.

Static analysis must never be described as runtime tracing or successful playback. Do not execute historical DOS binaries as part of this initial static-analysis phase. Do not modify converter behavior to compensate for unexplained offsets or failed comparisons.

## Next work

1. Identify and hash the exact MIMPIV4 executable used by each previous Ghidra export; compare it with the current private copy before trusting any old function address.
2. Reconstruct SNG2S v1–v4 parsing and event emission from the original executable bytes, checking every claimed function boundary and data flow independently.
3. Reconstruct MIMPI V4.27's direct SNG reader and playback scheduler independently, beginning with the raw MZ image and relocation handling rather than imported function names.
4. Build version-by-version structural ledgers from the complete SNG_DATA corpus and paired SNG2S MIDI references.
5. Only after the format and behavior ledgers are coherent, design the Glosa converter and direct player as separate implementations.
6. Add regression tests that compare decoded events, absolute ticks, tracks, SysEx, controller/program/pitch-bend data, metadata, and end-of-track positions—not merely successful parsing or event counts.

No SNG converter or player code has been changed in this baseline commit.

## Playback architecture audit

The current Glosa playback path is SMF-centric:

- `Playback/SongFiles.cs` recognizes `.mid`, `.midi`, `.rmi`, `.rcp`, `.r36`, `.g18`, and `.g36`; it does not include `.sng`.
- `PlaybackController` defaults to loading each item with `SmfReader.Read(path)` and stores the result as a `MidiSequence`.
- `Sequencer` consumes a `MidiSequence` and sends its events to an `IEventSink`.
- The CLI's documented `play` command accepts MIDI files; the only SNG-specific CLI command is `convert-sng`.

Therefore, the current repository has no direct SNG playback reader or MIMPI-compatible SNG playback path. Adding direct playback is a separate feature, not a small extension to the existing SMF converter. The existing sequencer and output sinks may still be reusable after the MIMPI event/timing model is understood.

## Reference track-order cross-check

The paired reference SMFs inspected for v1–v4 contain a conductor track followed by a SysEx-only track, then the part tracks. Their part-name order is consistent with the SNG2S manual's default descending order, including the v4 insertion of parts 16 down through 11 between parts 10 and 9.

The current converter's `for (part = 1; part <= Parts; part++)` traversal emits parts in ascending order and has no SNG2S option parameter. This is a separate compatibility gap from the missing SysEx-only track. The exact options used to create every private reference MIDI are not yet established, so the manual's default-order rule and each sample's observed order must remain separately recorded.

## Executable identity notes

The inspected SNG2S executable is also an MZ DOS executable. Its file length is 32,383 bytes, its MZ header is 512 bytes, and its header contains seven relocation entries. Its embedded version text identifies SNG2S Ver. 3.3.

The current private-repository tree for MIMPIV427 contains the manual, executable, and font file, but no checked-in Ghidra export or function-analysis text. Earlier handoff documents refer to an export and a different historical path. Until that export's source executable is recovered and byte-compared with the current MIMPIV4.EXE, its function names, addresses, and relocation coverage remain provisional.

## Representative reference-event counts

A byte-level SMF scan of one paired SNG2S reference from each version produced the following counts. These are sample baselines, not universal version limits:

| Version | Note-ons | Note-offs | Conductor tempo events | SysEx events in dedicated track | SMF tracks |
|---|---:|---:|---:|---:|---:|
| v1 | 3,295 | 3,295 | 3 | 4 | 12 |
| v2 | 2,713 | 2,713 | 424 | 17 | 12 |
| v3 | 5,362 | 5,362 | 10 | 41 | 12 |
| v4 | 1,721 | 1,721 | 24 | 224 | 18 |

The v2–v4 tempo counts show why a single initial-tempo assertion is not an adequate test. The SysEx counts also show that SysEx generation is not a small optional corner case in the representative later-version files.

## MIMPI V4.27 raw-code re-check: timer queue versus tempo stream

A first independent disassembly pass was made from the current MIMPIV4.EXE bytes, not from Ghidra function names. The MZ header is 160 paragraphs (0xA00 bytes). Under the assumed load-image base segment 0x1000, a segmented address is mapped to file offset by:

`file_offset = 0xA00 + (segment - 0x1000) * 16 + offset`

This mapping must still be checked against the exact base used by the earlier Ghidra project.

The code around file offset 0x11D80 disassembles coherently when aligned from CS:141F:

- CS:141F begins a routine with `push bp`.
- CS:1429 clears DS:0440.
- CS:142F loads CX from DS:7C10; CS:1433 stores it to DS:0442.
- CS:1437 performs a far call to 0FF6:1431.
- CS:143C calls CS:1B1A, then returns at CS:143F–1440.

With the mapping above, the historical address 1FF5:1430 lands inside the instruction beginning at CS:142F, not at an instruction boundary. Therefore the earlier label of 1FF5:1430 as a callback entry is not currently substantiated. Resolve the original Ghidra image-base / segment mapping and exact input executable before reusing that address.

Adjacent code at CS:1446 checks DS:0434 against 1, then scans up to 64 four-byte records beginning at DS:3432. It decrements each record's first word as a countdown. When a countdown expires, it passes the record's +2 and +3 bytes, followed by zero, to the byte-output routine at CS:5E24. A separate flush routine at CS:1DB6 explicitly constructs a status byte as `0x80 | (record[+2] & 0x0F)`, sends record[+3] as the data byte, and sends zero as the final byte. This is strong evidence that this particular table is a pending note-off timer queue, not a tempo-value table.

The code at CS:1441 also jumps indirectly through CS:[0x140C]. Earlier code at CS:1404 writes 0x34B1 into that slot, so the dispatch target is runtime-modified; the static bytes initially present at CS:140C are not the final target. This is another reason not to infer function boundaries from a static function listing alone.

**T-016 correction boundary:** the DS:3432 four-byte countdown records must not be identified as SNG tempo records. Their observed consumer emits note-off-shaped MIDI bytes. The actual MIMPI SNG tempo-record reader and its path into the playback timer remain unidentified. This does not prove that MIMPI ignores tempo records.

These are static observations from the current executable. No runtime trace, DOS execution, or audible playback was performed.

## Cross-validated SNG2S / MIMPI tempo and volume streams

The current executable bytes allow a stronger comparison than the earlier Ghidra function labels.

### Tempo stream

In SNG2S, the scheduler code around CS:350A obtains a record stream through ES:[0x21E0] and advances its cursor in four-byte steps. In MIMPI, the corresponding code around CS:1526 obtains a stream through ES:[0x6E8C] and also advances in four-byte steps.

For both paths, the observed record shape is:

- offset +0: a countdown/delay word;
- offset +2: a tempo parameter word;
- a parameter value at or above 0xFF ends the stream;
- the countdown is decremented by the scheduler before the next tempo change is applied.

Both implementations calculate the timer count as:

`timerCount = clamp(floor(baseTempo * tempoParameter / 64), 1, 480)`

SNG2S then emits an SMF tempo meta event when the timer count changes. Its observed conversion is:

`microsecondsPerQuarter = floor(60000 / timerCount) * 1000`

The assembly performs integer division by 64, clamps to 1..480, divides 60,000 by the resulting count, multiplies by 1,000, and emits `FF 51 03` plus the resulting 24-bit value.

MIMPI applies the same timer-count calculation to its playback timer state instead of writing an SMF tempo event. Its corresponding code stores the computed interval in DS:0442 and passes the interval to the timer-state helper at CS:0E04.

The base-tempo value is not universally fixed at 120. SNG2S loads its base value into DS:2962 from version-dependent internal header state (the v1 and later-version paths use different fields). MIMPI loads DS:7C10 from a version-dependent header structure: its version flag DS:7820 is set to 0, 1, 2, or 3 for SNG version characters '1', '2', '3', or '4'; the base-tempo byte is read at structure offset +0x1C for flag 0 (v1), and +0x06 for flags 1–3 (v2–v4). The mapping from that internal structure to the original file byte offset still needs to be established.

The representative reference samples independently agree with the arithmetic:

- v1: base 120 and parameter 37 produce timerCount 69 and 869,000 µs/qn; the reference also has an exact 869,565 µs/qn event at tick 192.
- v2: base 60 and parameter 64 produce timerCount 60 and 1,000,000 µs/qn at tick 128.
- v3: base 120 and parameter 89 produce timerCount 166 and 361,000 µs/qn; the reference has this at tick 92 and an exact 361,445 µs/qn event at tick 192.
- v4: base 120 and parameter 123 produce timerCount 230 and 260,000 µs/qn at tick 116.

This cross-match strongly confirms the shared tempo-record semantics and the coarse tempo conversion. The separate path that emits exact microseconds-per-quarter at the first bar has not yet been fully traced in the executable.

### Volume stream

SNG2S uses ES:[0x21E4] and MIMPI uses ES:[0x6E90] for a separate four-byte stream. Both read the +0 countdown and +2 value, clamp values at or above 0x65 to 0x4B, and update the volume path. MIMPI's downstream path emits MIDI controller 7 (Channel Volume). Therefore ES:[0x6E90] is a volume automation stream, not the tempo stream.

### Separate note-off timer queue

The SNG2S 64-entry queue at DS:0x0FA0 and the MIMPI 64-entry queue at DS:0x3432 are also distinct from the tempo and volume streams. Their records count down and emit note-off-shaped messages. Do not conflate these three different record queues.

These cross-executable matches are based on static disassembly of the current SNG2S and MIMPIV4 binaries. They do not yet establish every version-specific file offset or the exact startup/bar-boundary tempo path.
