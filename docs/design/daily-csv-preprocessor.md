# daily-csv-preprocessor Design

## Overview
daily-csv-preprocessor は、CSV Processing Pipeline の前処理を担当するツールです。

日次で出力されるRaw CSVファイルを1ファイル単位で読み込み、カラム内改行の補正、必要カラムの抽出、不足カラムの補完を行い、後続の monthly-latest-processor で扱いやすい前処理済みCSVとして出力します。

本ツールでは、複数ファイルの結合および record_id + updated_at による最新レコード抽出は行いません。

---

## Purpose
日次CSVファイルを後続処理で扱いやすい形に整形することを目的とします。

主な目的は以下です。
- カラム内改行によるCSV崩れを補正する  
- target_columns.csv に基づき必要カラムのみを抽出する  
- 指定カラムが存在しない場合は空列として補完する  
- 前処理済みCSVを年月フォルダに出力する  
- 再実行しても同じ結果になるようにする（冪等性）  

---

## Process
処理の流れは以下の通りです。
1. 設定情報を読み込む  
2. 対象となる日次CSVファイルを取得する  
3. target_columns.csv を読み込む（flag = TRUE のカラムを抽出）  
4. 日次CSVファイルを1ファイルずつ処理する  
5. カラム内改行を補正する  
6. CSVを読み込む  
7. 必要カラムのみを抽出する  
8. 不足カラムを補完する（存在しない場合は空列追加）  
9. 前処理済みCSVを出力する  
10. ログを出力する  

---

## I/O

### Input
- Raw daily CSV（UTF-8 / CSV）  
- 主キー：record_id  
- 更新日時：updated_at  
- target_columns.csv  

例：

| column_name | flag |
|-------------|------|
| record_id   | TRUE |
| updated_at  | TRUE |
| address     | FALSE|

---

### Output
前処理済みCSV（Preprocessed daily CSV）
- 必要カラムのみを含む  
- 不足カラムは空列として補完済み  
- カラム内改行補正済み  

出力構造：
output_root/   202604/     daily_data_20260401_preprocessed.csv     daily_data_20260402_preprocessed.csv

---

## Tech Stack
- Python  
- pandas  
- DuckDB  
- CSV  

---

## Notes

### Responsibility
本ツールは日次CSVの前処理のみを担当します。

以下は対象外です：

- 複数CSVファイルの結合  
- 月次データ作成  
- 最新レコード抽出  
- 集計・可視化  

---

### Column Selection
出力対象カラムは target_columns.csv で管理します。

- コード変更なしでカラム制御可能  
- 存在しないカラムは空列で補完  

---

### Error Handling
- 読込失敗ファイルはログ出力してスキップ  
- 指定カラムが存在しない場合は空列追加  
- 改行補正失敗時はログ出力  
- 出力先フォルダが存在しない場合は作成  

---

### Idempotency（冪等性）
同じ入力と設定で実行した場合、同じ結果になる設計とします。
- 出力ファイルは上書き  
- 再実行しても結果は変わらない  

---

### Performance
- 不要カラムは早期に除外  
- 1ファイル単位処理でメモリ抑制  
- 必要に応じてDuckDBを利用  

---

## Future Extension
- エラーレコードの退避  
- 処理ログのCSV出力  
- 設定ファイル拡張  
- 複数文字コード対応  
- DuckDBによる高速化
