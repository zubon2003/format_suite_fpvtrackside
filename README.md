# format_suite_fpvtrackside

FPVドローンレース用タイミングソフト **[FPVTrackside](https://github.com/mattyt/FPVTracksideCore)** 向けの、レースフォーマット（組み合わせ表）集です。

予選（Qualify）の順位をもとに決勝トーナメントを走らせるための **マルチクラス・ダブルエリミネーション** フォーマットを、参加人数ごとに用意しています。FPVTrackside に読み込ませる `formats/` の xlsx と、当日の掲示・手作業運用のための印刷配布用 xlsx の2種類が入っています。

## 収録フォーマット

### multclass_double_elimination

23〜32人規模のマルチクラス・ダブルエリミネーション。4ch（4機同時飛行）前提です。

| 参加人数 | クラス数 | FPVTrackside 用フォーマット | 印刷配布用 |
| --- | --- | --- | --- |
| 23人 | 2クラス | `formats/2_class_de_23 person.xlsx` | `(23-24人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 24人 | 2クラス | `formats/2_class_de_24 person.xlsx` | `(23-24人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 25人 | 2クラス | `formats/2_class_de_25 person.xlsx` | `(25-26人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 26人 | 2クラス | `formats/2_class_de_26 person.xlsx` | `(25-26人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 27人 | 2クラス | `formats/2_class_de_27 person.xlsx` | `(27人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 28人 | 2クラス | `formats/2_class_de_28 person.xlsx` | `(28人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 29人 | 2クラス | `formats/2_class_de_29 person.xlsx` | `(29人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 30人 | 2クラス | `formats/2_class_de_30 person.xlsx` | `(30人)配布印刷・手作業用2classDoubleElimination.xlsx` |
| 31人 | 3クラス | `formats/3_class_de_31 person.xlsx` | `(31-32人)配布印刷・手作業用3classDoubleElimination.xlsx` |
| 32人 | 3クラス | `formats/3_class_de_32 person.xlsx` | `(31-32人)配布印刷・手作業用3classDoubleElimination.xlsx` |

印刷配布用ファイルが人数レンジ（23-24人など）でまとめられているのは、その範囲であればトーナメント表の枠組みが同一で、欠員分を空欄のまま運用できるためです。FPVTrackside に読み込ませるフォーマットのほうは、人数ごとに個別のファイルを使ってください。

いずれのフォーマットも全5回戦＋A決勝戦（計6ラウンド）構成です。

- **2クラス（23〜30人）**: 1回戦 → 2回戦 → 3回戦 → 4回戦 → 5回戦 → A決勝戦。B決勝戦の位置は人数により異なります（23-24人は4回戦、25〜30人は5回戦）。
- **3クラス（31〜32人）**: 4回戦がB決勝戦・C決勝戦、5回戦を経てA決勝戦。

## 使い方

### 1. FPVTrackside でフォーマットを使う（`formats/`）

1. 該当人数の xlsx を、FPVTrackside の**フォーマットフォルダ**（イベントの `Formats` ディレクトリ）にコピーします。
2. FPVTrackside でイベントを開き、予選（Qualify）ラウンドを走らせて順位を確定させます。
3. Rounds 画面からフォーマットを適用すると、シートの定義どおりに各ラウンドの組み合わせが生成されます。

シートの構造は FPVTrackside のフォーマット仕様に準拠しています。

- `Channels` = 4（1レース4機）、`Lock Channels` = False
- `Pilots` 列が予選順位（1位＝1行目）
- `Round Race N` 列に各ラウンドのレース組み合わせ、`Results` 列に結果参照

1回戦は予選順位の直接参照（`=$B$18` など）、2回戦以降は前ラウンドの着順を `INDEX`/`MATCH` で引く数式になっているため、結果が入るたびに次のレースの出走者が確定します。

### 2. 印刷・手作業運用で使う（配布印刷用 xlsx）

各ファイルは3シート構成です。

| シート | 用途 |
| --- | --- |
| `予選順位入力` | B列に予選順位どおりにパイロット名を貼り付ける入力シート |
| `決勝トーナメント自動入力シート` | 名前と着順から次レースの出走者を自動計算する運用シート |
| `決勝トーナメント(印刷配布掲示用）` | 名前を入れない、枠だけの掲示・配布用シート |

手順:

1. `予選順位入力` シートのB列に、予選1位から順にパイロット名を入力（貼り付け）します。
2. `決勝トーナメント自動入力シート` の1回戦に自動で反映されます。
3. 各レースの左側の細い枠に着順（1〜4）を入力すると、次のレースの出走者が自動で埋まります。
4. `決勝トーナメント(印刷配布掲示用）` シートは「予選17位」「R1-1 １位」のような枠だけの表記なので、事前に印刷して会場掲示や配布に使えます。

FPVTrackside を使わずに手作業で運営する場合や、PCトラブル時のバックアップとしても利用できます。

## 動作環境

- FPVTrackside（フォーマット読み込みに対応したバージョン）
- Microsoft Excel、LibreOffice Calc など xlsx の数式を扱える表計算ソフト

Google スプレッドシートでも開けますが、数式の再計算や書式が崩れる場合があるため、印刷配布用は Excel / LibreOffice での利用を推奨します。

## ライセンス

[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)（パブリックドメイン提供）

著作権および関連する権利を放棄しています。許諾表示なしで、複製・改変・配布・商用利用を自由に行えます。全文は [LICENSE](LICENSE) を参照してください。
