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
