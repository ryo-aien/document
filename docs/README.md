# 技術ドキュメント

このリポジトリは、機械学習、自然言語処理（NLP）、Retrieval-Augmented Generation（RAG）に関する技術ドキュメントをまとめたものです。

## 📁 ディレクトリ構造

### fundamentals/ - 基礎知識
機械学習とNLPの基礎的な概念やモデルに関するドキュメント

#### 01_機械学習基礎/
- `ml-fundamentals.md` - 機械学習の基本概念、学習/推論アルゴリズム、実行環境の総覧
- `ml-libraries-guide.md` - 主要な機械学習ライブラリの比較と選定ガイド

#### 02_NLP/
- `nlp-models-catalog.md` - NLPモデルアーキテクチャのカタログ（Encoder/Decoder/Seq2Seq）
- `rag-architecture.md` - RAGシステムのアーキテクチャ設計と実装ガイド
- `japanese-tokenization.md` - 日本語形態素解析の概要とツール比較

### infrastructure/ - インフラ
ハードウェアとクラウド環境に関するドキュメント

#### 03_ハードウェア/
- `cpu-gpu-processing.md` - CPU/GPUの処理フローとCUDA実行モデル
- `gpu-selection-guide.md` - GPU性能比較と用途別選定ガイド
- `accelerator-comparison.md` - CPU/GPU/TPUとCUDAの技術比較

#### 04_クラウド/
- `cloud-gpu-pricing.md` - クラウドGPUサービスの料金比較と月額シミュレーション

### development/ - 開発
機械学習モデルの開発と最適化に関するドキュメント

#### 05_開発/
- `llm-evaluation.md` - LLMの精度評価手法とベンチマーク
- `rag-metrics.md` - RAG/検索システムの評価指標（Recall@k等）
- `lora-fine-tuning.md` - LoRAを用いた効率的なファインチューニング手法
- `task-specific-models.md` - タスク別特化モデルの総合カタログ
- `training-optimization.md` - 深層学習の学習速度向上テクニック
- `gpt5-coding-cheatsheet.md` - GPT-5活用のコーディングチートシート
- `08_fine-tuning.ipynb` - ファインチューニングの実装例（Jupyter Notebook）

### experiments/ - 実験・検証
各種検証実験の結果と分析

#### 07_検証実験/
- `01_多言語RAG性能比較/` - 日本語と英語のRAG性能比較実験
  - `01_日本語英語RAG比較評価.md` - 実験結果と考察
  - `02_日本語英語RAG比較実験.ipynb` - 実験用コード
- `02_RAG_Embeddingモデル性能検証/` - Embeddingモデルの性能検証
  - `01_モデル性能比較_概要.md` - 性能比較の概要
  - `02_モデル別検索精度詳細結果.md` - 詳細な検証結果
  - `03_RAGモデル性能比較実験コード.ipynb` - 実験用コード
- `03_形態素解析/` - 形態素解析に関する実験
  - `形態素解析.ipynb` - 実験用コード

### platforms/ - プラットフォーム固有
各AIプラットフォームに特化したドキュメント

#### 08_OPEN_AI/
- `openai-resources.md` - OpenAI公式リソースとドキュメントへのリンク集
- `openai-safety-policy.md` - OpenAI APIセーフティチェックポリシー

#### 09_AIagent/
- `ai_agent_report.pdf` - AIエージェントに関するレポート

#### 10_Claude/
- `claude_code_cli_command_reference.md` - Claude Code CLIコマンドリファレンス
- `claude_dir_structure.md` - Claudeディレクトリ構造の説明
- `claude_settings_spec.md` - Claude設定仕様
- `claude_cli_tools_flow.md` - CLIツールフロー

### references/ - リファレンス
外部リソースやリンク集

#### 99_link/
- `platform-docs.md` - AI/MLプラットフォーム公式ドキュメントリンク集

### assets/ - 共有リソース
ドキュメント全体で使用する画像などのリソース

#### images/
- 各種ダイアグラムやフロー図（SVG形式）

---

## 🚀 使い方

各カテゴリごとにトピックが整理されています。関心のある分野のディレクトリから探索してください。

- **基礎から学びたい**: `fundamentals/` から開始
- **実装を始めたい**: `development/` を参照
- **インフラ構築**: `infrastructure/` を確認
- **ビジネス検討**: `business/` をチェック
- **実験結果を見る**: `experiments/` を参照

## 📝 ドキュメント形式

- **Markdown** (`.md`) - 技術ドキュメント
- **Jupyter Notebook** (`.ipynb`) - コード実装例と実験
- **PDF** - レポートや詳細資料
- **PowerPoint** - プレゼンテーション資料

## 🔄 更新履歴

ドキュメントは継続的に更新されます。最新の情報については各ファイルの更新日時を確認してください。
