# Archive review inventory

Status: initial inventory; review in progress  
Last checked: 2026-11-04 (follow-up comparison)  
Repository: `pikachu364/glosa`  
Base snapshot: `main` at `4c7e123953bb04ff6ff354da4af78dc0e93a9f80`

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

The last fetched branch list showed `development` at the same SHA as main (`24681a45430bb93bf47098f42b0adb1387c84dcd`). It was created accidentally during an earlier operation. Do not use it for development. Its deletion is a separate cleanup decision and is not performed by this inventory PR.

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
