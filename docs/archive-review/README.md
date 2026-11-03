# Archive review inventory

Status: initial inventory; review in progress  
Last checked: 2026-11-04  
Repository: `pikachu364/glosa`  
Base snapshot: `main` at `24681a45430bb93bf47098f42b0adb1387c84dcd`

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
- Inspect the unique historical log material in `docs/trace-tempo-branch-state-2026-10-26`.
- Inspect the remaining unique material in `reverse/v1-tempo-investigation` and `fix/ballade-v1-layout-2026-10-22` only where main does not already contain a later record.
- Finish classifying v1-v4 fixture/data assets and their provenance.
- Keep the main-to-upstream reset/synchronization out of scope until documentation and preservation review is complete.
