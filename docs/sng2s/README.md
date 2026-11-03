# SNG2S conversion investigation

## Goal

Record independently written technical notes about the historical SNG2S utility and define a non-destructive MIDI comparison process. Audio playback is not required for structural inspection.

## Scope

SNG2S is a historical PC-98/MS-DOS utility for converting Music-kun, Music Rou, and Ballade SNG data to Standard MIDI Files. The reviewed version 3.3 materials describe Ballade 3 support and conversion of selected SysEx data.

The original executable, archive, documentation files, and reference MIDI remain in local research storage. None are included in this repository. A successful conversion of one input does not establish event-level correctness, identify which converter is responsible for a difference, or prove playback equivalence.

## Relevant options

The reviewed version 3.3 documentation describes these comparison-relevant options:

- `/0` and `/1`: MIDI Format 0 or Format 1; Format 1 is the documented default.
- `/F`: ascending SNG track order; without it, documented Ballade output ordering is reversed.
- `/N`: omit tracks without note data.
- `/MT`: suppress module-off SysEx messages.
- `/X1`: place SysEx in the conductor track; `/X2` is the documented default and uses a dedicated SysEx track; `/XD` disables SysEx conversion.
- `/U`: map supported non-Ballade-3 PCM parts to MIDI channels 11–16 without channel-change SysEx. The documentation says this does not apply to Ballade 3.
- `/LA`: map PCM tones to similar MT-32 tones and omit PCM-selection SysEx.
- `/R`: disable running status.
- `/DEF`: create a MIMPI sound-definition file.
- `/O`: select the output directory.

The documented MIDI timebase is fixed at 48 ticks per quarter note. Existing output files may be overwritten, so comparisons should use a separate output directory and preserve inputs.

## Version notes

The reviewed version 3.3 readme describes Ballade 3 support from version 3.0, Track Name meta events (`FF 03`) from version 2.0 onward, and a version 3.3 fix for Ballade 3 tracks 11 and above when `/N` is used. It explicitly warns against versions 3.0–3.1 due to incorrect conversions.

A filename or accompanying readme alone does not prove the executable's identity. Verify the binary version when a reliable, non-destructive method is available. Track-name metadata is a useful inspection point, not a complete correctness test.

## Comparison dimensions

Inspect these independently: MIDI format, track count, time division, tempo events and absolute ticks, note-on/off events, program changes, channels, controllers, pitch bend, SysEx content and placement, meta events, end-of-track positions, and duration.

Module-off SysEx can silence a different sound module. `/MT` suppresses those messages but does not resolve tone-number differences. Track order, running status, and byte encoding can differ without changing the decoded musical event stream.

## Investigation plan

1. Parse the reference MIDI without playback and record format, tracks, division, tempo, event counts, note ranges, controllers, programs, SysEx, meta events, and end-of-track timing.
2. Inspect track-name/title metadata for `FF 03` and note any `FF 01` events.
3. Compare output from another converter only when it uses the same SNG input; compare decoded events and absolute ticks.
4. Separate semantic event differences from track-layout and byte-encoding differences.
5. Record exact tool version, input identity, options, and output identity.
6. Keep source inputs read-only and write generated files elsewhere.

## Current status

- [x] Reviewed version 3.3 option descriptions and comparison-relevant behavior.
- [x] Recorded version-history details relevant to Ballade 3 and MIDI metadata.
- [x] Defined comparison dimensions and a non-destructive workflow.
- [ ] Produce a structural report for the local reference MIDI.
- [ ] Compare against another converter using the same SNG input.
- [ ] Document reproducible findings and limitations.

## Repository boundary

Keep original third-party executables, archives, documentation files, private fixtures, and reverse-engineering dumps outside this public repository unless redistribution rights and provenance have been reviewed. This file is an independently written technical summary, not a copy of the original utility documentation.
