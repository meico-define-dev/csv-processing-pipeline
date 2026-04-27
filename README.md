# Daily CSV Preprocessor

## Overview
日次で出力されるCSVファイルを結合・加工・整形するツールです。  
Power Queryで行っていた処理をPythonに移行し、再利用性と保守性の向上を目的としています。
業務での利用を想定したツール開発を進めています。

## Features
- 複数CSVファイルの結合
- データのクレンジング（不要列削除・型変換）
- 重複データの除外
- 更新日時ベースで最新データを抽出

## Tech Stack
- Python
- DuckDB
- Power Query（移行元）
- ChatGPT（設計・実装支援）

## Status
開発中（in progress）

## Author
Meico

## Usage
### ①　入力データ
日次で出力されるCSVファイルを指定フォルダに配置

### ②　実行
Pythonで以下を実行

```bash
python main.py
```
※ 実行環境： Python 3.x
※ 事前に必要なライブラリ：
- pandas
- duckdb

### ③　出力
加工済みのCSVファイルを指定フォルダに出力

## Process
以下の処理を行います。

1. CSVファイルを読み込み
2. 必要なカラムを抽出・型変換
3. 更新日時を基準に重複データを整理（最新データのみ保持）
4. 加工済みデータを出力
