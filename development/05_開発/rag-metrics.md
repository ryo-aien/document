# 🎯 Recall@k

## ✅ 説明

Recall@k は、検索結果の上位 k 件の中に、必要な正解文書がどれだけ含まれているかを測定する指標です。Retriever が必要な情報を取り漏らさず取得できているか（再現性）を評価。
ユーザーの探している情報が見つかっているか。
## 🧮 式

```
Recall@k = (Top-k内に含まれる正解文書の数) / (正解文書の総数)
```

## 🔍 例

* 正解文書：`[doc3, doc7]`
* 検索結果Top-5：`[doc1, doc3, doc9, doc5, doc8]` → Recall@5 = 1 / 2 = **0.5**

## 💡 特徴

* 検索結果に必要な文書が漏れなく含まれているかを測る
* RAGやFAQ検索などで Retriever の網羅性を評価
* Precision@k とセットでバランス評価に使う
* 正解文書が複数ある場合でも有効
* k を増やすと値は上がりやすい

## 🛠 実装

```python
def recall_at_k(retrieved_docs, relevant_docs, k):
    retrieved_top_k = retrieved_docs[:k]
    hits = len(set(retrieved_top_k) & set(relevant_docs))
    return hits / len(relevant_docs)
```


# 🎯 Precision@k

## ✅ 説明

Precision@k は、検索結果の上位 k 件の中に、どれだけ正解文書が含まれているかを測る指標。Retriever が無関係な文書をどれだけ除外できているか（精度）を評価。
表示された結果に、無関係な情報が少ないこと。

## 🧮 式

```
Precision@k = (Top-k内に含まれる正解文書の数) / k
```

## 🔍 例

* 正解文書：`[doc3, doc7]`
* 検索結果Top-5：`[doc1, doc3, doc9, doc5, doc8]` → Precision@5 = 1 / 5 = **0.2**

## 💡 特徴

* 検索結果の質（ノイズの少なさ）を測る
* Recall@k とセットでバランス評価に使う
* 正解文書が少ないタスクではスコアが低くなりがち
* 小さいkほど厳しい評価になる

## 🛠 実装

```python
def precision_at_k(retrieved_docs, relevant_docs, k):
    retrieved_top_k = retrieved_docs[:k]
    hits = len(set(retrieved_top_k) & set(relevant_docs))
    return hits / k
```


# 🎯 MRR（Mean Reciprocal Rank）

## ✅ 説明

正解文書が検索結果の何番目に現れたかの逆数で評価。早く現れるほど高評価。探していた答えがTop-kの上位に出ること。

## 🧮 式

```
MRR = 1 / rank（正解文書が最初に登場する順位）
```

## 🔍 例

* 正解文書：`[doc3]`
* 検索結果：`[doc1, doc3, doc5]` → MRR = 1 / 2 = **0.5**

## 💡 特徴

* 「最初に見つかる正解」の順位重視
* 順位に敏感、上位に出ることを重視した評価
* 複数正解がある場合、最上位の1件のみを評価対象にする

## 🛠 実装

```python
def mrr(retrieved_docs, relevant_docs):
    for rank, doc_id in enumerate(retrieved_docs, start=1):
        if doc_id in relevant_docs:
            return 1.0 / rank
    return 0.0
```



# 🎯 Hit@k

## ✅ 説明

Top-kの中に1つでも正解文書が含まれていれば1、含まれていなければ0。
Top-kの中に正解が含まれていること。

## 🧮 式

```
Hit@k = 1（正解あり） or 0（正解なし）
```

## 🔍 例

* 正解文書：`[doc7]`
* 検索結果Top-3：`[doc1, doc2, doc7]` → Hit@3 = **1**

## 💡 特徴

* 単純かつ直感的な評価指標
* リトリーバーの最低限の性能チェックに有効

## 🛠 実装

```python
def hit_at_k(retrieved_docs, relevant_docs, k):
    return int(bool(set(retrieved_docs[:k]) & set(relevant_docs)))
```

# 🎯 Exact Match（EM）

## ✅ 説明

生成された文が正解文と完全一致するか（文字単位）。
模範解答とまったく同じ回答が返ること。

## 🧮 式

```
EM = 1（完全一致） or 0（不一致）
```

## 🔍 例

* 出力: `"返品は30日以内に可能です"`
* 正解: `"返品は30日以内に可能です"` → EM = **1**

## 💡 特徴

* 非常に厳格な評価
* 定型的な応答が求められるQAに向いている
* 表現のゆれには弱い

## 🛠 実装

```python
def exact_match(prediction, reference):
    return int(prediction.strip() == reference.strip())
```

# 🎯 F1スコア（token-based）

## ✅ 説明

トークン（単語）レベルで出力と正解の重なりを評価する。部分一致に寛容。
正解と出力の重要な単語のトークンがほぼ一致すること

## 🧮 式

```
F1 = 2 * (Precision * Recall) / (Precision + Recall)
```

## 🔍 例

* 出力: `"返品は30日以内に可能"`
* 正解: `"30日以内なら返品可能"` → 共通トークンが多ければ F1 は高い

## 💡 特徴

* EM より柔軟で実用的
* QAや生成タスクでよく使われる
* 表現の違いがあっても一致度を測れる

## 🛠 実装

```python
def f1_score_token_level(prediction, reference):
    pred_tokens = prediction.strip().split()
    ref_tokens = reference.strip().split()
    common = set(pred_tokens) & set(ref_tokens)
    
    if not common:
        return 0.0
    
    precision = len(common) / len(pred_tokens)
    recall = len(common) / len(ref_tokens)
    return 2 * precision * recall / (precision + recall)
```


# 🎯 ROUGE / BLEU

## ✅ 説明

出力文と参照文の n-gram 一致度（ROUGE: 要約、BLEU: 翻訳）。
要約などでどれだけ内容が一致し、文法や語順が正しいか。
要点が押さえられ、自然・正確こと。


## 🧮 式

* ROUGE: n-gram recall（特にROUGE-1, ROUGE-L）
* BLEU: n-gram precision（1〜4-gramの重み付き）

## 🔍 例

* 出力: `"返品は30日以内"`
* 正解: `"30日以内に返品可"` → n-gram が共通していれば高スコア

## 💡 特徴

* 要約や長文生成でよく使われる
* ROUGEは recall 寄り、BLEUは precision 寄り
* 表現の違いにはやや弱い

## 🛠 実装

```python
# ROUGE
from rouge_score import rouge_scorer
scorer = rouge_scorer.RougeScorer(['rouge1', 'rougeL'], use_stemmer=True)
scores = scorer.score("生成文", "参照文")

# BLEU
from nltk.translate.bleu_score import sentence_bleu
score = sentence_bleu([reference.split()], prediction.split())
```

#### ※n-gram（エヌグラム） とは、テキストを連続する n個の単語や文字の並び。nには、整数が入る。文字区切り、単語区切りがある。

# 🎯 BERTScore

## ✅ 説明

BERT埋め込みベースで文と文の意味的な類似度を評価。意味的評価に強い。
違う表現だが、出力と正解の意味が近いこと。

## 🧮 式

```
cosine(埋め込み(生成文), 埋め込み(参照文))
```

## 🔍 例

* 出力: `"返品可能"`
* 正解: `"返金に応じます"` → 表現は異なっても意味が近ければ高スコア

## 💡 特徴

* 意味的評価に最適、パラフレーズに強い
* 実行には `bert-score` パッケージが必要

## 🛠 実装

```python
from bert_score import score

P, R, F1 = score([prediction], [reference], lang="ja")
print(F1[0].item())
```

# 🎯 LLM Judge

## ✅ 説明

GPT-4などの大規模言語モデルを使って、回答の有用性や正確性を主観的に評価。LLM視点での全体的な品質。

## 🧮 式

```
自然言語ベースの判断 → スコアリングやA/B選択で集計
```

## 🔍 例

```
質問: 「返品ポリシーは？」
回答A: 「30日以内なら返品可能です」
回答B: 「返品は受け付けていません」
→ GPTに「どちらが適切か」を判断させる
```

## 💡 特徴

* 自動評価が難しい応答・要約・創造的出力に有効
* 柔軟で人間的な評価が可能（信頼性・自然さなど）
* 実行にはGPT-4などのAPIが必要（コストも発生）

## 🛠 実装（プロンプト例）

```python
prompt = f"""
質問: {question}
回答A: {answer_a}
回答B: {answer_b}

どちらの回答が質問に対してより正確で有用かを選んでください。理由も添えてください。
"""
```

#####  ※Top-kとは、検索の意味が近い上位k件のこと。