# チャット引継ぎ — Glosa SNG 対応 / MIMPIV4.EXE 追加解析

更新日: 2026-11-02

## 1. 目的と現在の調査対象

Glosa の Ballade SNG 対応に関する逆解析を継続する。現在の重点は **T-016 — MIMPIV4 の legacy mode-1 再生時 tempo-record 処理**。今回、Ghidra の関数一覧・デコンパイル出力だけでなく、元の MIMPIV4.EXE の MZ ヘッダー、relocation table、entry point、末尾の追加領域を直接調べ始めた。

- Glosa: https://github.com/pikachu364/glosa
- 逆解析文書: https://github.com/pikachu364/glosa/tree/main/docs/reverse
- private 参照データ: https://github.com/pikachu364/glosa-private-data/tree/main/testdata/ballade/MIMPI427
- 対象 EXE: [MIMPIV4.EXE](https://github.com/pikachu364/glosa-private-data/blob/diagnose-ghidra-function-coverage-2026-11-02/testdata/ballade/MIMPI427/MIMPIV4.EXE)
- 対象 EXE SHA-1: `e6b1bbe5a3684a814ce5127a2ead89c7e3199daa`
- 対象 EXE サイズ: 136,903 bytes

## 2. GitHub の状態

- PR #52 `docs: locate MIMPIV4 mode-1 callback target` は **2026-11-02 に merge 済み**。
- merge commit: `020d1dc6377ab8776ad180b1e2a035b1f0b7e983`
- PR: https://github.com/pikachu364/glosa/pull/52
- PR #52 は FACTS / NEXT_STEPS / LOG の文書変更であり、converter・テスト・公開 fixture・private 解析データは変更していない。
- 今回の引継ぎ更新は新しいブランチ `docs/mimpiv4-exe-analysis-handoff-2026-11-02` で行う。PR 作成後も、ユーザーが明示的に指示するまでマージしない。

## 3. 既存文書に記録済みの T-016 の結論

main の `README.md` に記載された F-153〜F-155 の要点:

1. legacy mode-1 の callback は `CS:[0x1406]` 経由で `1FF5:1430` に分岐する。
2. 既存の private Ghidra text export には `1FF5:1430` の関数本体・命令一覧が含まれていない。
3. 元 EXE の静的逆アセンブルでは、同アドレスに実行コードが存在する。
4. そのコードは stream record の `+2` にある 16-bit 値を `0x7BF0 / 64` でスケーリングし、結果を 1..480 に制限して `0x0440` を更新する。その後 `0x043E` を加算し、`FUN_1FF5_0DEE` に渡す経路が確認されている。
5. これは mode-1 tempo-record consumer の有力な候補。ただし record field の意味を確定するには、実際の v1 `runner.SNG` の該当 record と照合する必要がある。
6. runtime trace / 実再生による確認は未実施。

**重要:** 上記は静的解析で確認された処理経路である。SNG record の +2 field の意味や、全ファイルに共通する仕様、実行時の timer 挙動まで確定したという意味ではない。

## 4. 今回の元 EXE 直接調査で確認したこと

### 4.1 MZ ヘッダー

- ファイル先頭は `MZ`。
- MZ header size: 160 paragraphs = 2,560 bytes。
- MZ declared module size: 125,203 bytes。
- entry point: `CS:IP = 0000:D016`。ファイルオフセットは `0xDA16`（55,830 bytes）。
- relocation count: 626。
- relocation table offset: 30。
- `e_ovno = 0`。
- `0x3C` の DWORD は通常の PE `e_lfanew` としてはファイル範囲外を指し、標準的な PE header は確認できていない。offset `0x06AC` にも `NE` signature は確認できていない。
- 以上は DOS MZ 形式と16-bit DOS 起動コードを強く支持する。ただし compiler version、packer、build process は特定していない。

### 4.2 entry point の初期命令

entry point `0xDA16` の先頭 bytes は次の通り:

```
b4 30 cd 21 3c 02 73 02 cd 20 bf c6 17 8b 36
02 00 2b f7 81 fe 00 10 72 03 be 00 10 fa 8e
d7 81 c4 fe 7b fb 73 10 16 1f e8 f7 01 33 c0
50 e8 76 04 b8 ff 4c cd 21
```

16-bit x86 として読むと、先頭は `mov ah,30h; int 21h` による DOS version 取得、DOS version 2 未満の場合の終了処理などと整合する。起動コードの全体的な意味は未解析。

### 4.3 relocation table

- relocation entry は 626 件。
- segment 別件数: segment 0 が 518 件、segment 4085 が 101 件、segment 6090 が 4 件、segment 7651 が 3 件。
- relocation の linear offset 範囲は 24〜122,424。MZ declared module の範囲内。
- 多数の load-time address fixup が存在することは確認できた。
- 各 relocation site の word 値を解釈し、コード・データ領域や overlay と対応付ける作業は未完了。segment 別件数だけから用途を断定しない。

### 4.4 MZ declared module の後ろにある 11,700 bytes

- MZ declared module size 125,203 bytes に対し、ファイル全体は 136,903 bytes。
- したがって、declared module の後ろに 11,700 bytes が存在する。
- 追加領域先頭は ASCII `MIMPIV4 D`。
- 先頭付近の little-endian 16-bit 値:
  - offset `0x0C`: `0x0FE4` = 4068
  - offset `0x0E`: `0x0DA0` = 3488
  - offset `0x10`: `0x1D84` = 7556
  - offset `0x12`: `0x0E82` = 3714
- これらの値が offset / length / resource index / その他の値のどれに該当するかは未確定。
- 4値の合計は 18,826 で追加領域 11,700 bytes を超えるため、単純な4領域の連続 length とみなすことはできない。
- 現時点では font / image / sound / executable code 等の種類を特定できていない。overlay / resource-like data という表現も仮説に留める。

### 4.5 文字列と runtime library

EXE 内に次の文字列を確認:

- `MS Run-Time Library - Copyright (c) 1988, Microsoft Corp`
- `MIMPI Ver%s%s for PC-9801`
- `Send MTD`, `Send CM6`, `Send GSD`
- `I/F NOT READY !`
- `Song file does not exist.`
- `Out of memory !`
- `Can not open 'MIMPI.MIX'`
- `Welcome MIMPI world`

Microsoft C runtime の文字列は存在するが、これだけで compiler version や正確な build toolchain を特定することはできない。

## 5. 解析データの取得状況と制約

private repository の対象:

- EXE: `testdata/ballade/MIMPI427/MIMPIV4.EXE`
- Ghidra script: `testdata/ballade/Ghidra/MIMPIV427.java`
  - SHA-1: `dac5fde175a3c4f6cdd461067b5c6cd28f74e116`
  - Ghidra version: 12.1.4
- 解析テキスト: `testdata/ballade/MIMPI427/mimpiv427解析データ.txt`
  - 既知サイズ: 2,588,668 bytes
  - SHA-1: `b828ca25f68b36b26984882319a0f243ee6e5cc6`

Ghidra export について過去に確認された値:

- Functions exported: 572
- String-like data exported: 504
- Decompiler errors: 0
- Cancelled: false

ただし、今回の connector 経由では上記解析テキストを取得できなかった。UTF-8 の行範囲指定でも空内容となり、base64 取得も 0 bytes だった。以前のローカル出力サイズ 2,588,676 bytes と repository 上のサイズには 8 bytes の差がある。したがって、以前の 572 function export と現在の repository ファイルが完全に同一とは扱わない。今回その全内容を再読・照合できたとは主張しない。

以前、同一内容の V4/V5 export は 2,588,689 bytes・108,750 lines で、出力ファイル名以外は同等だったとの記録がある。V6 の 572 functions / 504 string-like data / decompiler errors 0 という結果も、今回の repository file の内容を独立に検証した結果ではない。

## 6. 確認済み・推測・未解決の区分

### 確認済み

- MIMPIV4.EXE は MZ header を持ち、entry point に 16-bit DOS API 呼び出しを含む。
- MZ header の declared module size と実ファイルサイズには 11,700 bytes の差がある。
- relocation entry は 626 件。
- entry point は file offset `0xDA16`。
- Microsoft runtime library の copyright string が存在する。
- mode-1 callback の `1FF5:1430` に実行コードが存在する。
- そのコードに stream record +2 の値を処理して `0x0440` を更新し、`0x043E` を加えて `FUN_1FF5_0DEE` に渡す静的経路がある（main README / F-153〜F-155）。

### 推測・未確定

- 末尾 11,700 bytes の正確な用途。
- 追加領域先頭の4つの16-bit値の意味。
- relocation segment 別分布が示す具体的なコード／データ区分。
- record +2 field が実際の v1 SNG 仕様上どの値・単位を表すか。
- EXE の compiler version、packer、build process。
- runtime における tempo update / timer scheduling の実挙動。

### 実施していないこと

- MIMPIV4 の runtime trace。
- MIMPIV4 の実再生による確認。
- 全572関数の最新 repository export との再照合。
- converter / tests の変更。
- private fixture や private analysis dump の public repository へのコピー。

## 7. 次に行う作業（優先順）

1. **PR と文書状態の確認:** この引継ぎを含む PR の head / CI / diff を確認する。PR #52 は merge 済みであり、再度マージ対象にしない。
2. **末尾領域の構造解析:** overlay-relative offset `0x0FE4`, `0x0DA0`, `0x1D84`, `0x0E82` の各位置を調べる。先頭付近の16-bit値が参照する位置なのか、サイズなのかを、実際の byte pattern と境界で検証する。値の意味を先に決めつけない。
3. **relocation site の対応付け:** 各 relocation entry の load-module-relative offset を file offset `2560 + segment*16 + offset` に変換し、該当 word と周辺 bytes を調べる。特に segment 4085 の101件、6090の4件、7651の3件を優先する。segment 分布のみからコード／データの意味を推測しない。
4. **entry point の逆アセンブル:** `0xDA16` から起動コードを連続して解析し、DOS version check、初期化、main処理への移行を特定する。逆アセンブル結果と推測を区別する。
5. **T-016 の record 照合:** mode-1 callback target `1FF5:1430` の record +2 field の読み取り元を、private v1 `runner.SNG` の実データと照合する。必要な byte-level 根拠、record 境界、値の endian / scale / clamp を記録する。private fixture bytes は public repository に掲載しない。
6. **関数 export の取得問題:** 解析テキストの repository サイズ・SHA と、以前の V6 local export のサイズ差8 bytesを解消できる場合だけ再取得を試す。既に空応答だった同じ方法を繰り返すだけにしない。
7. 証拠が揃ったら FACTS.md の既存 F-153〜F-155 と重複しない形で事実を追記し、HYPOTHESES.md / NEXT_STEPS.md / README.md / LOG.md の関連箇所を必要な範囲で更新する。ID は既存最大値の次を使用し、既存の記述を読み替えずに履歴を残す。
8. converter・テスト・公開 fixture は、T-016 の完了条件と参照データ照合が満たされるまで変更しない。

## 8. GitHub 作業ルール

- 文書・コード変更とも main へ直接 commit しない。ブランチを作成して PR で提案する。
- PR の CI / safety check が失敗した場合はマージせず、失敗原因を確認して修正する。
- CI が成功しても自動マージしない。マージはユーザーの明示的な指示を待つ。
- 既存 PR の状態を先に確認し、同じ内容を重複して追加しない。
- private repository の EXE、SNG/MID fixture、Ghidra export、解析 dump の実データを public repository にコピーしない。public docs には必要最小限の解析結果・offset・説明のみ記録する。
- FACTS は確認済み事実と根拠・制約、HYPOTHESES は未確定の説明候補、NEXT_STEPS は実施する作業と完了条件、LOG は時系列の作業履歴、README は現在の全体結論を記録する。
- 既存の節や task の途中に関係する内容を追記・修正する。Next-step だけを繰り返し追加して、既存の結論や未解決事項を更新しない運用にしない。

## 9. 次のチャット開始時の指示

「Glosa SNG 対応の T-016 を引き継ぎます。まずこの引継ぎ文書と main の FACTS / README / NEXT_STEPS、および関連 PR の状態を確認してください。PR #52 は merge 済みです。MIMPIV4.EXE は MZ header、entry point、626 relocation、末尾 11,700 bytes の存在まで確認済みですが、末尾領域の用途と先頭4値の意味は未確定です。まず末尾領域の4値が指す候補位置と relocation site を実データで検証してください。mode-1 の `1FF5:1430` は tempo-record consumer の有力候補ですが、record +2 の意味を v1 runner.SNG と照合し、runtime trace / 実再生は未実施として扱ってください。private fixture / dump を公開せず、文書更新はブランチ＋PR、マージは明示指示があるまで行わないでください。」

---

**最重要:** MIMPIV4 の末尾追加領域を font / image / sound 等と断定しないこと。mode-1 の record +2 を tempo 値と断定する前に v1 SNG の実 record と照合すること。静的解析結果を runtime trace / 実再生済みと書かないこと。
