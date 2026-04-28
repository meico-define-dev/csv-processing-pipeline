# CSV Processing Pipeline

## Overview
日次で出力されるCSVファイルを前処理し、月次単位で結合・最新化するツールです。  
Power Queryで行っていた処理をPythonに移行し、再利用性と保守性の向上を目的としています。

## Architecture
```text
Raw daily CSV
↓
daily-csv-preprocessor
↓
Preprocessed daily CSV
↓
monthly-latest-processor
↓
Monthly latest CSV
```

## Components
### daily-csv-preprocessor
日次CSVファイル単位の前処理を担当します。
- カラム内改行の補正
- 必要カラムの抽出
- 不足カラムの補完
- 前処理済みCSVの出力

### monthly-latest-processor
前処理済みCSVを月次単位で結合し、最新レコードを抽出します。
- 複数ファイルの結合
- record_id単位で最新レコード抽出（updated_at）
- 月次latest CSVの出力

## Tech Stack
- Python
- pandas
- DuckDB
- Power Query（移行元）
- ChatGPT（設計・実装支援）

## Status
開発中（in progress）

## Author
Meico
