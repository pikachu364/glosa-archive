# Ballade SNG Version Test Data

This directory contains the public Ballade SNG input fixtures used by Glosa's version-specific converter tests.

## Test fixtures

| File | Reported Ballade version | Size | SHA-256 |
|---|---:|---:|---|
| `v1/runner.SNG` | 1.00 | 85,897 bytes | `e61b8b0315a3752dfa3d3546d5a436df0d4b0421201183c21ce4bf1780556e15` |
| `v2/SHIZUME0.SNG` | 2.00 | 75,597 bytes | `eb6e1eb5bd1aeb36acb7b53a89fb8409b09e7d3988a45847309da105622272ee` |
| `v3/START__0.SNG` | 3.00 | 82,391 bytes | `b4b719cec15139e411c94c34b05ca698e9432b9ccad308a808f08a648256cbe4` |
| `v4/BOURREE0.SNG` | 4.00 | 49,517 bytes | `a1eb69b4735b809e35c9c368776e9dae621e30405dc5a9c717133654c5f7b715` |

The version labels are the reported versions of these samples. They do not imply that Glosa can automatically identify every version from its file header.

See [`manifest.json`](manifest.json) for machine-readable fixture metadata and validation scope.

## What the public CI verifies

The `Glosa.Tests` test project is run by the repository's cross-platform GitHub Actions CI on Windows, macOS, and Linux.

The checked-in Ballade tests currently verify:

- Each of the four public SNG fixtures can be converted by `SngConverter`.
- Each generated SMF can be read back by `SmfReader` and contains at least two tracks.
- For the v1 `runner.SNG` fixture, the generated tempo-event byte sequences include the expected values at ticks 0, 8, and 192 (500,000, 869,000, and 869,565 microseconds per quarter note).
- The v1 `runner.SNG` conversion produces 3,295 Note-on and 3,295 Note-off events. This is a count check, not an event-by-event comparison.
- A v1 no-change tempo-stream case retains the default 120 BPM tempo.

These checks establish the behavior covered by the tests; they do not establish complete format compatibility or playback equivalence.

## Reference comparisons and private test data

Reference MIDI files, private-only fixture tests, and comparison tooling are maintained separately in the private companion repository:

[`pikachu364/glosa-private-data`](https://github.com/pikachu364/glosa-private-data)

They are intentionally not copied into this public repository. A successful historical private-fixture CI run applies only to the Glosa commit named in that run; it must not be treated as validation of a later `main` commit without rerunning the workflow against that commit.

The public tests do not currently establish byte-for-byte equality with reference SMF files, complete event-level equality for every version, or actual playback equivalence.

## Remaining validation work

- Run the private fixture workflow against the current Glosa `main` when reference-comparison results are needed.
- Keep the public fixture metadata and private fixture manifest consistent without exposing private binary data.
- Record unsupported structures and any differences found by the reference comparisons.
- Implement automatic version identification only when the file format provides a reliable discriminator.
