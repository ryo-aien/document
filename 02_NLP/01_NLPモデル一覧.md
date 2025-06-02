# NLP(自然言語処理)モデル一覧

主なモデルファミリーと代表例。  
「Encoder-only」「Decoder-only」「Encoder–Decoder (Seq2Seq)」の３種類。  
どのタスクに対し、どのモデルが適しているか。

---

## 1. Encoder-only（入力文を埋め込みに変換して分類など）
- **BERT 系**
  - BERT (Bidirectional Encoder Representations from Transformers)
  - RoBERTa (Robustly optimized BERT approach)
  - ALBERT (A Lite BERT；パラメータ共有による軽量化)
  - DistilBERT (知識蒸留による軽量版)
  - ELECTRA (判別器を用いた事前学習)
- **XLNet 系**
  - XLNet (双方向＋自己回帰の組み合わせ)
- **DeBERTa 系**
  - DeBERTa (Disentangled attention による性能向上)

### 用途例
- 文書分類、感情分析、命名体認識（NER）、QA（質問応答）のリランキングなど  
- 入力文全体を双方向（前後文）から学習して、高精度な文脈埋め込みを得るのに適している

---

## 2. Decoder-only（自己回帰型：文章生成や対話に向く）
- **GPT 系**
  - GPT-2 / GPT-3 / GPT-3.5 / GPT-4 など
  - GPT-Neo / GPT-J（EleutherAI のオープンソース版）
  - LLaMA (Meta 社の軽量版大規模言語モデル)
  - Vicuna / Alpaca（LLaMA を微調整した対話特化モデル）
- **OPT 系**
  - OPT (Meta が公開したオープンソースの大規模言語モデル)
- **Bloom 系**
  - BLOOM (多言語対応の大規模言語モデル、BigScience コラボレーション)

### 用途例
- 自然言語生成（テキスト生成、要約、ストーリーテリング）  
- チャットボットや対話システム（プロンプト＋履歴を与えて応答を生成）

---

## 3. Encoder–Decoder (Seq2Seq：入力文を符号化し、それを基に新しい文を生成)
- **T5 系**
  - T5 (Text-to-Text Transfer Transformer；あらゆるタスクをテキスト生成タスクとして扱う)
  - mT5（多言語対応版）
- **BART 系**
  - BART (Bidirectional and Auto-Regressive Transformers；文の破損復元＋生成タスクで事前学習)
  - MBART（多言語 BART）
- **Pegasus 系**
  - PEGASUS (要約タスクに特化して事前学習されたモデル)
- **MarianMT**
  - Marian (Helsinki-NLP による機械翻訳モデル群)
- **ByT5 / ByT5-small**
  - バイトレベルでトークン化を行う T5 派生版

### 用途例
- テキスト要約、機械翻訳、質問応答（QA）、対話生成（エンコーダで入力を理解し、デコーダで応答・翻訳文を生成）

---

## 4. 文脈埋め込み・検索向けモデル
- **Sentence-BERT 系**
  - Sentence-BERT (双方向のBERTを Siamese network 構造で学習し、文ベクトルを取得)
  - SBERT 市場版（multi-task 学習版など）
- **LaBSE**
  - LaBSE (Language-agnostic BERT Sentence Embedding；多言語横断検索向け)
- **MiniLM**
  - MiniLM (軽量 Sentence-BERT 派生版)
- **USE（Universal Sentence Encoder）**
  - Google 提供の文埋め込みモデル（TensorFlow Hub 版）

### 用途例
- 意味検索、文書クラスタリング、類似度計算、FAQ 検索エンジンのバックエンドなど

---

## 5. 特殊タスク向けモデル
- **LayoutLM / LayoutLMv2/3**
  - 文書（PDF やスキャン画像）のレイアウト情報を組み込んで理解する
- **Vision-Language モデル**
  - CLIP (画像とテキストを一緒に埋め込む)
  - BLIP (画像キャプションやビジュアル QA など)
- **音声テキスト変換（ASR）／音声認識モデル**
  - Whisper (OpenAI が公開した音声→テキスト変換モデル)
  - Wav2Vec 2.0（Facebook 提案の音声特徴抽出＋識別モデル）
- **マルチモーダル対話モデル**
  - VL-BART, VisualGPT など、画像や音声コンテキストを含む対話生成向けモデル

---

## 6. 軽量化・蒸留・圧縮モデル
- **DistilBERT / TinyBERT / MobileBERT**
  - 知識蒸留や構造変更で小型化した BERT 系モデル
- **Quantized モデル**
  - INT8 などに量子化した軽量版
- **Pruned モデル**
  - パラメータプルーニングで冗長性を削った版

### 用途例
- エッジデバイスやスマホ、組み込み環境で高速推論や省メモリを実現したい場合

---

# まとめ
- **Encoder-only**: 文を高精度に理解して分類・抽出したい場合に使う（例：BERT, RoBERTa）。  
- **Decoder-only**: テキスト生成や対話応答に特化（例：GPT 系, LLaMA）。  
- **Encoder–Decoder**: 入力文を理解しつつ別文を出力する要約・翻訳・QA 系で使う（例：T5, BART, Pegasus）。  
- **埋め込み生成**: 意味検索や類似度計算を目的とした軽量モデル（例：Sentence-BERT, LaBSE）。  
- **特殊タスク**: 文書構造解析やマルチモーダル処理など、特定用途に特化したファミリーも多数存在。  
- **軽量化モデル**: モバイル・エッジデバイス対応などで蒸留・量子化した派生版が豊富。  
