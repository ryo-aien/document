
# 学習高速化

##  概要

本ドキュメントでは、深層学習における学習速度向上のための具体的な手法を、以下の2ケースに分けて整理します

* 小〜中規模モデル × 単一GPU環境
* 大規模モデル × 複数GPU（分散学習）環境

---

## パターン①：小〜中規模モデル × 単一GPU

### 想定環境

* モデル：BERT-base、ResNet50、LSTM等（〜数億パラメータ）
* ハード：単一GPU
* 案件：文書分類、画像認識、小規模ファインチューニングなど

---

### 高速化の構成要素

| 項目                             | 内容                                                            |
| ------------------------------ | ------------------------------------------------------------- |
|  `torch.compile()`           | PyTorch 2系。モデルの実行グラフを最適化し自動高速化。                               |
|  Mixed Precision（AMP）        | `torch.cuda.amp` を使用して fp16/bf16 による高速・低メモリ学習                 |
|  SDPA Attention               | `torch.nn.functional.scaled_dot_product_attention` を使いFlash対応 |
|  DataLoader最適化              | `num_workers`、`prefetch_factor` を増やしてI/Oを並列処理                 |
|  学習率スケジューラ                   | warmup + cosine decay でより高速に収束                                |
|  勾配累積（gradient accumulation） | 小バッチで大バッチ相当の効果                                                |

---
##  パターン②：大規模モデル × 複数GPU（分散学習）

### 想定環境

* モデル：GPT系、LLM、ViT-Largeなど（10億パラメータ以上）
* ハード：マルチGPU
* 案件：大規模事前学習、LLMファインチューニングなど

---

### 高速化の構成要素

| 項目                                   | 内容                                      |
| ------------------------------------ | --------------------------------------- |
|  `torch.compile()`                 | 自動グラフ最適化（特に大規模モデルでは効果大）                 |
|  Mixed Precision（AMP/bfloat16）     | float16 or bfloat16 精度で学習・メモリ削減         |
|  Flash Attention 2                  | TransformerのAttentionを高速実装に差し替え         |
|  FSDP（Fully Sharded Data Parallel） | GPU間でモデルの重み・勾配を分散、メモリ効率最適化              |
| Gradient Checkpointing            | 中間計算を一部再計算することでメモリ節約（メガモデルに必須）          |
|  LoRA / Adapter学習                  | 少数パラメータだけを学習し、効率的なファインチューニングが可能         |
|  ZeRO Optimizer                    | DeepSpeed/FSDP経由で勾配や重みを細かく分散管理          |
|  モニタリング                            | Weights & Biases、TensorBoardでログ圧縮＋通信量削減 |

---

### 構成ツール例

* PyTorch FSDP
* Hugging Face Accelerate / Transformers
* DeepSpeed
* xFormers / Flash Attention 2
* NVIDIA Apex

---


## 比較表

| 項目                     | 単一GPU向け        | 複数GPU（分散学習）向け   |
| ---------------------- | -------------- | --------------- |
| torch.compile          | ◯              | ◎（効果大）          |
| Mixed Precision (AMP)  | ◯   | ◎ |
| Attention最適化（Flash等）   | ◯              | ◎               |
| FSDP / DeepSpeed       | ✕              | ◎（必須）           |
| Gradient Checkpointing | △ | ◎ |
| LoRA                   | ◯（小型モデルでも有効）   | ◎（巨大モデルで非常に有効）  |

---

##  まとめ

* 単一GPUでは、AMP・`torch.compile()`・効率的なDataLoaderが主な高速化ポイント。
* 複数GPU環境では、FSDPやFlash Attention、Checkpointing、LoRAなどを組み合わせて高速化＋省メモリを両立するのが現代の実践パターン。
