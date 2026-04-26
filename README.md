# Daily CSV Preprocessor

## Overview
日次で出力されるCSVファイルを結合・加工・整形するツールです。  
Power Queryで行っていた処理をPythonに移行し、再利用性と保守性の向上を目的としています。

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