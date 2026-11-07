# Archive review inventory

Status: initial inventory; review in progress  
Last checked: 2026-11-05 (post-PR #61 branch audit)  
Repository: `pikachu364/glosa`  
Base snapshot: `main` at `95b575dd799786f11d6df52d2483c3eab7d8451b`

## Purpose

This document is a temporary index for deciding which documentation, source files, test data, and branches should remain.

It is not an instruction to delete files or branches. No deletion is authorized by listing an item here. When uncertain, preserve the original branch and data.

The current development target is SNG v2-v4. v1 work is historical/reference material and must not be mixed into the current implementation without a separate decision.

## Decision rules

| Classification | Meaning | Action |
|---|---|---|
| KEEP | Needed by current development, tests, or maintained documentation | Keep in the normal project tree |
| ARCHIVE | Historical material that may be useful for v1/reference or future comparison | Preserve content and Git history where possible |
| MERGED | Already represented in main, or superseded by a later main document | Do not re-import; branch can be considered for cleanup separately |
| REMOVE-CANDIDATE | Apparently unnecessary, but not yet approved for deletion | Record evidence and wait for review |
| UNCONFIRMED | Content, provenance, or relationship is unclear | Preserve and investigate |

A branch being behind main does not, by itself, prove that its unique content is disposable. An ahead-by-zero comparison means its commits are reachable from main; it does not prove that every old file version is still present at the current path.

## Current keep / archive decisions

### KEEP — current main documentation and implementation

- `docs/DEVELOPMENT_HANDOFF.md`
- `docs/MACOS_RUNNER_POLICY.md`
- `docs/UPSTREAM_SYNC.md`
- `docs/reverse/` current investigation records, including `FACTS.md`, `HYPOTHESES.md`, `LOG.md`, `NEXT_STEPS.md`, `V1_TEMPO_INVESTIGATION.md`, and current handoff/comparison documents
- `docs/sng2s/README.md`
- Current v2-v4 implementation, tests, and public test fixtures already in main

These paths are not deletion targets in this review. The main documents may already contain later findings than older branch copies.

### ARCHIVE — preserve historical v1 material and its history

Do not delete or rewrite these branches as part of routine cleanup:

- `docs/ballade-version-testdata` — historical SNG/MIDI data, converter/test material, manifests, and work records. Preserve v1 through v4 comparison material. Do not merge the branch wholesale into main.
- `feature/midi-converter-validation` — v1-era MIDI comparison tool and its 11 unique commits. This is distinct from the standalone `pikachu364/sng-midi-validator` repository.
- `feature/ballade-sng` — v1-era implementation/documentation history; retain as a historical branch.
- `reverse/v1-tempo-investigation` — 33 unique investigation commits; retain even where main has later versions.
- `fix/ballade-v1-layout-2026-10-22` — historical v1 layout investigation.
- `docs/trace-tempo-branch-state-2026-10-26` — retain its historical investigation trail pending a final check of unique log details.

Also preserve `main/testdata/ballade` and `docs/ballade-version-testdata` contents. Do not move private fixtures, reference MIDI, or analysis dumps into a public repository without a separate provenance/privacy review.

### MERGED — no re-import expected based on current comparison

The following branches had no commits ahead of main in the previous comparison. Treat them as already represented in main for commit-history purposes; do not re-import them merely to make the branch list shorter.

- `docs/add-sng2s-investigation-guide-2026-11-03`
- `docs/audit-empty-part-state-drains-2026-10-27`
- `docs/ballade-layout-comparison-2026-10-21`
- `docs/chat-handoff-2026-11-02`
- `docs/clarify-sng2s-tempo-exit-2026-10-30`
- `docs/confirm-leading-empty-drain-2026-10-27`
- `docs/fix-v2-tempo-evidence-wording-2026-10-28`
- `docs/handoff-2026-11-01-t016`
- `docs/inspect-default-part-state-2026-10-27`
- `docs/mimpiv4-exe-analysis-handoff-2026-11-02`
- `docs/mimpiv4-mode1-043e-write-source-audit-2026-11-01`
- `docs/mimpiv4-mode1-driver-path-2026-11-01`
- `docs/mimpiv4-mode1-timer-offset-write-audit-2026-11-01`
- `docs/mimpiv4-tempo-base-field-trace-2026-10-30`
- `docs/reconcile-reverse-status-2026-11-03`
- `docs/record-sng-v2-tempo-merge-2026-10-30`
- `docs/refresh-ballade-fixture-validation-2026-11-03`
- `docs/resolve-later-tempo-grid-boundary-2026-10-30`
- `docs/resolve-v2-tempo-boundary-2026-10-27`
- `docs/sng2gs-source-alignment`
- `docs/t016-1430-export-gap-2026-11-02`
- `docs/t016-mode1-callback-target-2026-11-01`
- `docs/t016-mode1-dispatch-audit-2026-11-01`
- `docs/t016-mode2-event-scan-boundary-2026-11-01`
- `docs/t016-zbar-timer-boundary-2026-11-01`
- `docs/tempo-block-offsets-2026-10-26`
- `docs/tempo-branch-state-trace-2026-10-26`
- `docs/tempo-grid-early-return-2026-10-29`
- `docs/tempo-grid-return-scope-2026-10-30`
- `docs/tempo-parser-read-seek-2026-10-26`
- `docs/tempo-queue-drain-boundary-2026-10-26`
- `docs/tempo-queue-flush-boundary-2026-10-26`
- `docs/tempo-startup-count-arithmetic-2026-10-26`
- `docs/tempo-status-branch-counts-2026-10-26`
- `docs/trace-absolute-tick-accumulator-2026-10-27`
- `docs/trace-default-part-state-2026-10-27`
- `docs/trace-later-tempo-record-ticks-2026-10-28`
- `docs/trace-n-flag-2026-10-26`
- `docs/trace-part-state-initialization-2026-10-27`
- `docs/trace-part-status-2026-10-26`
- `docs/trace-tempo-grid-first-records-2026-10-26`
- `docs/trace-v2-base-tempo-source-2026-10-27`
- `docs/verify-v2-base-tempo-byte-2026-10-27`
- `fix/sng-v2-initial-tempo-tick-128-2026-10-30`
- `fix/sng-v3-v4-tempo-event-timing-2026-10-30`
- `fix-tempo-ledger`
- `refactor/move-sng-converter-to-sng-2026-10-30`
- `maintenance/xunit2031-assert-single`
- `test/sequencer-schedule-origin`
- `test/stabilize-split-sysex-stop-windows-2026-10-29`
- `test/stabilize-stop-silence-timing-2026-10-22`

This is a prior comparison snapshot. Recheck each branch head before deleting any branch.

## Special cases requiring separate review

| Branch | Prior comparison | Current handling |
|---|---:|---|
| `docs/upstream-sync-workflow` | 1 ahead; its `docs/UPSTREAM_SYNC.md` blob matched main | No re-import expected; verify history and then consider cleanup |
| `docs/trace-tempo-branch-state-2026-10-26` | 4 ahead | Preserve; inspect any unique historical log details before deciding |
| `feature/ballade-sng` | 5 ahead | ARCHIVE; do not delete |
| `feature/midi-converter-validation` | 11 ahead | ARCHIVE; do not delete |
| `fix/ballade-v1-layout-2026-10-22` | 3 ahead | ARCHIVE; do not delete |
| `reverse/v1-tempo-investigation` | 33 ahead | ARCHIVE; do not delete |
| `docs/ballade-version-testdata` | 68 unique commits in prior comparison | ARCHIVE; do not merge wholesale or delete |

### `development` branch

`development` was created accidentally during an earlier operation and is not used for development. On 2026-11-05 it pointed to `24681a45430bb93bf47098f42b0adb1387c84dcd`, while `main` pointed to `95b575dd799786f11d6df52d2483c3eab7d8451b`. It was 6 commits behind `main`, 0 commits ahead, with no file differences in the comparison. Do not use it for development. Its deletion is a separate cleanup decision and is not performed by this inventory PR.

## Branch cleanup policy

For ordinary task branches:

1. Make changes on a dedicated branch, never directly on main.
2. Open a pull request to main.
3. Check the complete diff, test results, security/safety implications, data provenance, and accidental inclusion of private fixtures or generated binaries.
4. If checks fail or the diff is unclear, do not merge.
5. After a successful merge, delete only that PR's task branch.
6. Keep explicitly archived historical branches listed above.

A branch that is already fully represented in main can be considered for deletion only after its head, unique files, and preservation requirements have been rechecked. This inventory does not itself delete branches.

## Safety and review checklist

- [ ] Confirm PR base is `main` and the head is the intended review branch.
- [ ] Inspect all changed paths; no unexpected source, fixture, binary, or generated file changes.
- [ ] Confirm no private reference MIDI, user data, secrets, tokens, or local machine paths are exposed.
- [ ] Confirm documentation does not present superseded hypotheses as current facts.
- [ ] Confirm v1 reference material remains preserved.
- [ ] Confirm CI / repository checks pass, or explain any checks that cannot run.
- [ ] Merge only after review; delete only the merged task branch.
- [ ] Do not delete archived branches or historical data as part of this PR.

## Remaining work

- Re-fetch branch heads and compare unique commits before any cleanup.
- Complete a final content-level review of archived v1 branches only if new evidence appears; current comparison found no confirmed current findings requiring re-import.
- Finish classifying v1-v4 fixture/data assets and their provenance.
- Keep the main-to-upstream reset/synchronization out of scope until documentation and preservation review is complete.


## Follow-up comparison — 2026-11-04

Compared the archived investigation branches against main at `4c7e123953bb04ff6ff354da4af78dc0e93a9f80`.

### `reverse/v1-tempo-investigation`

- The branch remains 33 commits ahead and 618 behind main; retain it as historical Git history.
- Its old FACTS headings F-051, F-057, F-058, F-060, F-061, and F-064 do not need wholesale re-import. Their subject matter is represented by later/current main records, including F-047, F-051, F-055, F-057, F-065, F-085, F-106, F-108, F-111, F-112, and F-116.
- The branch's older LOG and NEXT_STEPS entries describe investigation stages that have since been superseded or completed in main. No confirmed current finding requiring a separate content import was identified in this pass.
- `docs/MACOS_RUNNER_POLICY.md` is present in main; do not re-import it.

### `fix/ballade-v1-layout-2026-10-22`

- The branch remains 3 commits ahead and 289 behind main.
- Its only file-level difference is in `docs/reverse/LOG.md`; its unique dated entry is covered by later main investigation records. Retain the branch as historical v1 history; no re-import is currently justified.

### `docs/trace-tempo-branch-state-2026-10-26`

- The branch remains 4 commits ahead and 220 behind main.
- Its one branch-only LOG heading records the correction to the private SNG2S analysis-export source path and the unresolved status-byte/drain investigation.
- The resulting findings and limitations are represented by main F-125 and related current records. Do not copy the old LOG section wholesale; retain the branch as historical provenance.

### Decision

All three branches remain `ARCHIVE`. This comparison found no confirmed missing current documentation that needs to be copied into main. Preserve the branches and their commit histories; do not delete them.

The ordinary merged-task branch cleanup remains separate. Recheck branch heads and preserve exceptions before any deletion. This inventory update does not delete any branch or file.


## Follow-up: `docs/ballade-version-testdata` — 2026-11-04

Compared branch `docs/ballade-version-testdata` with main at `323afee0205cc3f0b37172113d0e27cd60057f16`.

- The branch has 68 commits ahead of main and 919 behind; it is a historical development/data branch, not a clean documentation-only branch.
- The comparison includes workflow changes, a historical SNG converter source file, converter tests, fixture documentation, manifests, reference MIDI, saved pre-fix Glosa MIDI, SNG2S documentation/archive, and dated work records. Do not merge the branch wholesale.

### Classification

| Material | Current classification | Reason / handling |
|---|---|---|
| Four public SNG input fixtures (v1-v4) | KEEP in main | Current main already has the four SNG input fixtures and SHA-256 values recorded in its current README/manifest. No duplicate copy needed. |
| Branch `testdata/ballade/README.md` | MERGED / superseded | Older README has sections for saved MIDI outputs, validation status, future work, and pending playback. Some facts are historical, but its status text is stale relative to current main (including v1 support and current CI assertions). Do not replace current main README with it. |
| Branch `testdata/ballade/manifest.json` | ARCHIVE only | Contains historical reference-output sizes and earlier verification claims, but also states v1 layout is unsupported and v1 is excluded. That is superseded by current main. Do not copy this manifest over main. |
| `v1/sng2gs/runner.MID` and v2-v4 `sng2gs/*.MID` | ARCHIVE / provenance-sensitive | Reference MIDI files are useful for future comparison. Keep them on the historical branch. Do not copy into main without confirming provenance, rights, and whether public distribution is intended. |
| v2-v4 `glosa-test-convert/*.mid` | ARCHIVE / historical outputs | These are explicitly described as pre-fix saved Glosa outputs. Preserve as historical comparison artifacts, not current expected outputs. |
| `testdata/ballade/sng2gs/SNG2S.DOC`, `README.DOC`, `sng2s33.lzh` | ARCHIVE / provenance-sensitive | Historical SNG2S documentation and archive may be valuable for v1 research. Version/options and redistribution rights are not established here. Preserve branch; do not import into main pending provenance review. |
| `testdata/ballade/作業記録/2026-10-19.md` | ARCHIVE | Historical work log with dated observations. Keep on the branch; do not treat its old conclusions as current facts. |
| `src/Glosa.Core/Smf/SngConverter.cs` | MERGED / superseded implementation history | The branch contains an older converter under the old namespace/path. Current main has the evolved implementation at `src/Glosa.Core/Sng/SngConverter.cs`, including the legacy v1 path. Do not copy the old file back. |
| `tests/Glosa.Tests/SngConverterTests.cs` | MERGED / superseded tests | Main has a later test suite using the current `Glosa.Core.Sng` namespace and newer v1-v4 assertions. Preserve branch history, but do not overwrite current tests with the older version. |
| Workflow changes | MERGED / verify separately if needed | The branch's CI concurrency addition is not a reason to restore its older workflow file over main. Current workflow policy differs. |
| 68 unique commits | ARCHIVE | Keep the branch and its Git history; it records the evolution of converter, tests, and comparison data. |

### Confirmed current-main state

- Main `testdata/ballade/README.md` describes the four public SNG fixtures, their SHA-256 values, current public CI scope, and explicitly distinguishes private/reference comparison data from public CI.
- Main `testdata/ballade/manifest.json` documents the current validation scope and limitations; it no longer says v1 is unsupported.
- Main converter is at `src/Glosa.Core/Sng/SngConverter.cs`, not the old branch path `src/Glosa.Core/Smf/SngConverter.cs`.
- Main test suite is `tests/Glosa.Tests/SngConverterTests.cs` and includes current v1 and v2-v4 fixture coverage.
- The branch's README/manifest contain historical comparison details, but their old status statements must not be presented as current facts.

### Decision

Keep `docs/ballade-version-testdata` as `ARCHIVE`; do not merge it wholesale and do not delete it. Keep current main fixtures/docs/tests as the authoritative current state. Retain reference MIDI, pre-fix outputs, SNG2S documents/archive, and the dated work log on the historical branch until provenance and future-use decisions are made. No file or branch deletion is authorized by this review.


## Follow-up branch audit — 2026-11-05

Base: `main` at `95b575dd799786f11d6df52d2483c3eab7d8451b` (after PR #61).
The branch list contained 60 branches and the open-PR list was empty at the time of review.

### Confirmed archived branches

| Branch | Ahead | Behind | Review result |
|---|---:|---:|---|
| `feature/midi-converter-validation` | 11 | 648 | ARCHIVE; retain v1 MIDI comparison tool and history; do not merge wholesale or delete |
| `feature/ballade-sng` | 5 | 925 | ARCHIVE; retain v1 implementation history; no current-doc re-import |
| `reverse/v1-tempo-investigation` | 33 | 622 | ARCHIVE; retain historical investigation; current main contains later findings |
| `fix/ballade-v1-layout-2026-10-22` | 3 | 293 | ARCHIVE; unique old LOG entry is covered by later investigation records |
| `docs/trace-tempo-branch-state-2026-10-26` | 4 | 224 | ARCHIVE; retain historical provenance; current facts/limitations are represented in main |

### Detailed review notes

- `feature/midi-converter-validation`: the branch-only files are the old local MIDI comparison CLI, four tests, README, dependency file, and validation note. Static review found that unmatched-note signatures omit channel and pitch, key-signature values are not compared, and some event-specific fields are absent from the comparison signature. The four tests do not cover these cases. The branch documentation says the tests and real-data validation were not yet run. Preserve this as historical v1 material; do not present its PASS result as complete MIDI equivalence.
- `feature/ballade-sng`: the branch's SNG2S note contains local reference-file details and a local MIDI hash; main has an independently written, narrower technical summary. Its extra Windows audio-device investigation note is environment-specific and records unresolved hypotheses. Do not re-import those sections into current main documentation.
- `reverse/v1-tempo-investigation`: the branch's `docs/MACOS_RUNNER_POLICY.md` blob matches main exactly. Its old v1 tempo notes are superseded by later main records; the current main `V1_TEMPO_INVESTIGATION.md` is longer and contains later validation boundaries. Retain the branch history, not the old document versions.
- `fix/ballade-v1-layout-2026-10-22`: its only file-level difference is a nine-line historical addition to `docs/reverse/LOG.md`; retain as provenance, no separate re-import identified.
- `docs/trace-tempo-branch-state-2026-10-26`: its unique FACTS/LOG/NEXT_STEPS additions describe an earlier investigation stage. Later main records contain the resulting findings and limitations. Retain the branch; do not copy the old log wholesale.
- `docs/upstream-sync-workflow`: one unique commit and 623 commits behind main, but its `docs/UPSTREAM_SYNC.md` blob is identical to main (SHA `109fe86a5d7d56db979bb6d276c22ffac0765a34`). No re-import is needed. It may be considered for ordinary cleanup only after a separate approval.
- `development`: 0 commits ahead and 6 behind main, with no file differences. It remains an accidental branch; no deletion was performed or authorized.

### Other branch comparisons

The following branches were confirmed as 0 commits ahead of main in the 2026-11-05 comparison; no re-import is indicated:

- `fix/sng-v2-initial-tempo-tick-128-2026-10-30`
- `fix/sng-v3-v4-tempo-event-timing-2026-10-30`
- `fix-tempo-ledger`
- `refactor/move-sng-converter-to-sng-2026-10-30`
- `maintenance/xunit2031-assert-single`
- `test/sequencer-schedule-origin`
- `test/stabilize-split-sysex-stop-windows-2026-10-29`
- `test/stabilize-stop-silence-timing-2026-10-22`
- `docs/sng2gs-source-alignment`

These comparisons establish commit reachability, not that every historical file version is present at the same path. Do not use ahead=0 alone as deletion authorization.

### Decision

Keep the explicitly archived v1 branches. Do not merge archived branches wholesale, do not delete any branch or file, and do not synchronize/reset main to upstream. Ordinary cleanup candidates (`development`, `docs/upstream-sync-workflow`, and fully represented task branches) require a separate review and explicit approval. The MIDI comparison CLI findings above are static code-review observations; tests were not executed in this audit.


## Full branch comparison after PR #62 — 2026-11-07

Base: `main` at `c1620d1d81391a6c62511fa37d09eb6d4a772582`.
The repository had 64 branches including main (63 compared against main), and no open PRs at the time of the snapshot.

### Comparison summary

- 56 branches: `ahead=0`; their tips are ancestors of current main. The comparison returned no file differences for these refs.
- 7 branches: diverged from main and require preservation/content review; details are below.
- An `ahead=0` result proves commit reachability, not that a branch's historical file contents still exist at the same path. It is evidence for cleanup candidacy, not deletion authorization.

### Diverged branches

| Branch | Ahead | Behind | Handling |
|---|---:|---:|---|
| `docs/ballade-version-testdata` | 68 | 923 | ARCHIVE; preserve all historical data and commits; no wholesale merge |
| `docs/trace-tempo-branch-state-2026-10-26` | 4 | 226 | ARCHIVE; preserve unique historical investigation trail |
| `feature/ballade-sng` | 5 | 927 | ARCHIVE; preserve v1 history |
| `feature/midi-converter-validation` | 11 | 650 | ARCHIVE; preserve v1 MIDI comparator and history |
| `fix/ballade-v1-layout-2026-10-22` | 3 | 295 | ARCHIVE; preserve v1 layout history |
| `reverse/v1-tempo-investigation` | 33 | 624 | ARCHIVE; preserve v1 investigation history |
| `docs/upstream-sync-workflow` | 1 | 625 | Its `docs/UPSTREAM_SYNC.md` blob is identical to main (SHA `109fe86a5d7d56db979bb6d276c22ffac0765a34`). One commit remains unique, but no current file-content difference was found. Cleanup candidate only after separate approval. |

The six v1/history branches above remain protected archive exceptions. The `docs/upstream-sync-workflow` branch has no missing current document content, but its unique commit history is not reachable from main; do not delete it without explicit approval.

### Ahead-zero branches

The following 56 branches were compared against the current main and returned `ahead=0` with no file differences:

- `development`
- `docs/add-sng2s-investigation-guide-2026-11-03`
- `docs/archive-review-ballade-data-2026-11-04`
- `docs/archive-review-followup-2026-11-04`
- `docs/archive-review-followup-2026-11-05`
- `docs/archive-review-inventory-2026-11-04`
- `docs/audit-empty-part-state-drains-2026-10-27`
- `docs/ballade-layout-comparison-2026-10-21`
- `docs/chat-handoff-2026-11-02`
- `docs/clarify-sng2s-tempo-exit-2026-10-30`
- `docs/confirm-leading-empty-drain-2026-10-27`
- `docs/fix-v2-tempo-evidence-wording-2026-10-28`
- `docs/handoff-2026-11-01-t016`
- `docs/inspect-default-part-state-2026-10-27`
- `docs/mimpiv4-exe-analysis-handoff-2026-11-02`
- `docs/mimpiv4-mode1-043e-write-source-audit-2026-11-01`
- `docs/mimpiv4-mode1-driver-path-2026-11-01`
- `docs/mimpiv4-mode1-timer-offset-write-audit-2026-11-01`
- `docs/mimpiv4-tempo-base-field-trace-2026-10-30`
- `docs/reconcile-reverse-status-2026-11-03`
- `docs/record-sng-v2-tempo-merge-2026-10-30`
- `docs/refresh-ballade-fixture-validation-2026-11-03`
- `docs/resolve-later-tempo-grid-boundary-2026-10-30`
- `docs/resolve-v2-tempo-boundary-2026-10-27`
- `docs/sng2gs-source-alignment`
- `docs/t016-1430-export-gap-2026-11-02`
- `docs/t016-mode1-callback-target-2026-11-01`
- `docs/t016-mode1-dispatch-audit-2026-11-01`
- `docs/t016-mode2-event-scan-boundary-2026-11-01`
- `docs/t016-zbar-timer-boundary-2026-11-01`
- `docs/tempo-block-offsets-2026-10-26`
- `docs/tempo-branch-state-trace-2026-10-26`
- `docs/tempo-grid-early-return-2026-10-29`
- `docs/tempo-grid-return-scope-2026-10-30`
- `docs/tempo-parser-read-seek-2026-10-26`
- `docs/tempo-queue-drain-boundary-2026-10-26`
- `docs/tempo-queue-flush-boundary-2026-10-26`
- `docs/tempo-startup-count-arithmetic-2026-10-26`
- `docs/tempo-status-branch-counts-2026-10-26`
- `docs/trace-absolute-tick-accumulator-2026-10-27`
- `docs/trace-default-part-state-2026-10-27`
- `docs/trace-later-tempo-record-ticks-2026-10-28`
- `docs/trace-n-flag-2026-10-26`
- `docs/trace-part-state-initialization-2026-10-27`
- `docs/trace-part-status-2026-10-26`
- `docs/trace-tempo-grid-first-records-2026-10-26`
- `docs/trace-v2-base-tempo-source-2026-10-27`
- `docs/verify-v2-base-tempo-byte-2026-10-27`
- `fix/sng-v2-initial-tempo-tick-128-2026-10-30`
- `fix/sng-v3-v4-tempo-event-timing-2026-10-30`
- `fix-tempo-ledger`
- `maintenance/xunit2031-assert-single`
- `refactor/move-sng-converter-to-sng-2026-10-30`
- `test/sequencer-schedule-origin`
- `test/stabilize-split-sysex-stop-windows-2026-10-29`
- `test/stabilize-stop-silence-timing-2026-10-22`

This includes the accidental `development` branch. Its current head is `24681a45430bb93bf47098f42b0adb1387c84dcd`, 8 commits behind main and 0 ahead. It has no file differences in the comparison. It is not used for development; deletion remains a separate decision.

The branches named `docs/archive-review-*` are historical PR/task refs. Their commits are reachable from main, but their branch pointers were not deleted in this audit.

### Cleanup decision

- Keep the six explicitly archived v1/history branches.
- Do not merge any diverged archive branch wholesale.
- Treat the 56 ahead-zero branches as cleanup candidates only after checking branch purpose, historical value, and retention exceptions.
- `docs/upstream-sync-workflow` is also a cleanup candidate because its only file blob matches main, but its one unique commit remains outside main history.
- No branch or file was deleted. No source, test, fixture, MIDI, or private data was modified.
- Do not synchronize/reset main to upstream until the archive/documentation review is complete and the user explicitly confirms.
