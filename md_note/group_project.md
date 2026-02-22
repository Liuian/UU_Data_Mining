# Assignment
## L1 penalty
### ⚖️ Characteristics of the L1 Penalty
* **Sparsity**
  → L1 regularization drives many weight values $w_i$ to exactly zero.
  → This effectively performs **automatic feature selection**.
* **Good for feature selection**
  → Especially useful for **high-dimensional data**, such as text classification or genomics.
* **Simpler and more interpretable model**
  → Because some coefficients become zero, the resulting model is easier to understand and analyze.

### 🧩 Comparison of Regularization Types
| Type | Name | Mathematical Form | Characteristics |
| ------ | ------ | ---------- | ----------- |
| **L1** | **Lasso Regularization** | $\lambda \sum w_i$ | Encourages sparsity — some weights become exactly zero, effectively performing feature selection. |
| **L2** | **Ridge Regularization** | $\lambda \sum w_i^2$ | Shrinks weights but does not make them zero — improves stability and prevents overfitting. |


## Logistic Regression
### 📈 Objective Function (Without Regularization)
Logistic Regression minimizes the following loss function, also known as the **negative log-likelihood**:
$$
L(w, b) = -\frac{1}{N} \sum_{i=1}^{N} [y_i \log(\hat{y}_i) + (1 - y_i)\log(1 - \hat{y}_i)]
$$

### 🔒 Adding the L1 Penalty to the Logistic Regression Loss
When adding an **L1 penalty**, the total loss function becomes:
$$
L_{\text{total}} = L(w, b) + \lambda \sum_i |w_i|
$$
where:
* $L(w, b)$: the original cross-entropy loss
* $\lambda$: the **regularization strength** (a larger λ means stronger penalty)
* $|w_i|$: the **absolute value of the weights**

## CountVectorizer & TfidfVectorizer
| 類別                | 名稱          | 主要功能                    | 適用情境                 |
| ----------------- | ----------- | ----------------------- | -------------------- |
| `CountVectorizer` | 詞頻向量化器      | 把每個文件轉成詞頻（每個詞出現的次數）     | 簡單基線模型（Bag-of-Words） |
| `TfidfVectorizer` | TF-IDF 向量化器 | 計算詞頻 × 逆文檔頻率（能降低常見詞的影響） | 文本分類、資訊檢索等高階任務       |


### 🧮 1️⃣ **CountVectorizer（詞頻向量化）**

`CountVectorizer` 是最簡單的文字向量化方式，也稱為 **Bag of Words（詞袋模型）**。
它只考慮「一個字詞在文件中出現的次數」。

#### 📘 公式：

對於某個文件 ( d ) 和詞彙表中的第 ( i ) 個詞 ( t_i )：

[
\text{Count}(t_i, d) = f_{t_i, d}
]

其中：

* $f_{t_i, d}$：詞 $t_i$ 在文件 $d$ 中出現的次數（frequency）

#### 📊 向量化後：

每篇文件 ( d ) 會變成一個向量：

$$
\mathbf{x}*d = [f*{t_1, d}, f_{t_2, d}, \dots, f_{t_n, d}]
$$

其中 ( n ) 是整個詞彙表的大小（vocabulary size）。

#### 🧠 範例：

假設詞彙表是 `[apple, banana, cat]`
文件內容是 `"apple banana apple"`

則：
$$
\mathbf{x} = [2, 1, 0]
$$
代表 apple 出現 2 次，banana 出現 1 次，cat 沒出現。

### 🧮 2️⃣ **TF-IDF Vectorizer（詞頻 × 逆文檔頻率）**

`TfidfVectorizer`（Term Frequency–Inverse Document Frequency）
會同時考慮詞在文件中的出現頻率（TF）以及詞在所有文件中的普遍程度（IDF）。

常出現的詞（像 “the”、“and”）會被降低權重。

#### ✳️ TF（Term Frequency）：

$$
\text{TF}(t, d) = \frac{f_{t,d}}{\sum_{t' \in d} f_{t',d}}
$$

* $f_{t,d}$：詞 $t$ 在文件 $d$ 中出現的次數
* 分母是該文件中所有詞的出現總數
  → 表示詞 $t$ 在文件中的相對重要性

#### ✳️ IDF（Inverse Document Frequency）：

$
\text{IDF}(t) = \log\left(\frac{N}{1 + n_t}\right)
$

其中：

* $N$：整個語料庫的文件總數
* $n_t$：包含詞 $t$ 的文件數
* 加上 1 是為了避免除以 0（平滑處理）

→ 常見詞會讓 $n_t$ 大，因此 IDF 小；稀有詞的 IDF 會大。

#### ✳️ 最終 TF-IDF 權重：

$$
\text{TF-IDF}(t, d) = \text{TF}(t, d) \times \text{IDF}(t)
$$

#### 📊 文件向量表示：

每篇文件 ( d ) 會變成一個向量：
$$
\mathbf{x}_d = [\text{TF-IDF}(t_1, d), \text{TF-IDF}(t_2, d), \dots, \text{TF-IDF}(t_n, d)]
$$

#### 🧠 小範例：

假設：

* 文件 1: `"apple banana apple"`
* 文件 2: `"banana cat"`

那麼：

| 詞      | 文件1 TF | 文件2 TF | 出現文件數 (n_t) | IDF             | TF-IDF(文件1) | TF-IDF(文件2) |
| ------ | ------ | ------ | ----------- | --------------- | ----------- | ----------- |
| apple  | 2/3    | 0      | 1           | log(2/1+1)=0.69 | 0.46        | 0           |
| banana | 1/3    | 1/2    | 2           | log(2/2+1)=0.0  | 0           | 0           |
| cat    | 0      | 1/2    | 1           | log(2/1+1)=0.69 | 0           | 0.35        |

---

### ⚖️ 總結比較

| 向量化方式 | 公式 | 特點 |
| ------- | ------- | --- |
| **CountVectorizer** | $f_{t,d}$ | 只考慮詞出現次數，簡單但可能被常見詞主導   |
| **TfidfVectorizer** | $\text{TF-IDF}(t, d) = \frac{f_{t,d}}{\sum f_{t',d}} \times \log(\frac{N}{1 + n_t})$ | 考慮詞在文件中及整體語料的權重，效果通常更好 |

## F1 Score
**F1-score（F1 分數）** 是在分類任務（classification）中非常常見的一個**模型評估指標**，
它用來綜合衡量模型的 **精確率（Precision）** 和 **召回率（Recall）**。

### 🧩 一、分類任務中的四種基本情況（Confusion Matrix）

當你做二元分類（Binary Classification）時，模型的預測結果可以分成四種情況：

| 實際值 \ 預測值        | **Positive (1)**        | **Negative (0)**        |
| ---------------- | ----------------------- | ----------------------- |
| **Positive (1)** | True Positive (**TP**)  | False Negative (**FN**) |
| **Negative (0)** | False Positive (**FP**) | True Negative (**TN**)  |

### 🎯 二、Precision（精確率）

**定義：在模型預測為正類的樣本中，有多少是真的正類？**

$$
\text{Precision} = \frac{TP}{TP + FP}
$$

> 「我預測是對的機率高不高？」

🔹 高 Precision → 錯誤預測為正的情況少
（例如垃圾郵件分類中，不想誤判正常信件為垃圾信）

### 🔍 三、Recall（召回率 / 敏感度）

**定義：在實際是正類的樣本中，有多少被模型成功找出來？**

$$
\text{Recall} = \frac{TP}{TP + FN}
$$

> 「我有沒有漏掉真正的正樣本？」

🔹 高 Recall → 能找到大部分真正的正樣本
（例如疾病檢測中，希望不要漏掉病人）

### ⚖️ 四、F1-score（F1 分數）

F1 是 **Precision** 和 **Recall 的調和平均（harmonic mean）**，
用來平衡兩者之間的權衡（trade-off）。

$$
\text{F1} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
$$

#### 💡 為什麼用「調和平均」？

因為調和平均會對低值比較敏感。
換句話說：

> 如果 Precision 或 Recall 其中之一很低，F1-score 也會很低。

這樣可以避免模型「只在其中一個指標好看」的情況。

### 🧠 五、舉個簡單例子

假設：

* TP = 80
* FP = 20
* FN = 40

那麼：

$$
\text{Precision} = \frac{80}{80 + 20} = 0.8
$$
$$
\text{Recall} = \frac{80}{80 + 40} = 0.667
$$
$$
\text{F1} = 2 \times \frac{0.8 \times 0.667}{0.8 + 0.667} \approx 0.727
$$

### 🧭 六、F1 分數的取值範圍

$$
0 \leq \text{F1} \leq 1
$$

* **1.0** → 模型完美（Precision 和 Recall 都是 1）
* **0.0** → 模型完全失敗

### 📊 七、何時使用 F1-score？

| 情境                      | 適不適合用 F1？        | 原因 |
| ----------------------- | ---------------- | -- |
| 資料集平衡（正負樣本數量差不多）        | ❌ 用 Accuracy 即可  |    |
| 資料集不平衡（正樣本很少）           | ✅ 用 F1-score 更合理 |    |
| 需要平衡 Precision & Recall | ✅ 非常適合           |    |

### ✅ 總結表

| 指標 | 公式 | 解釋 | 著重點 |
| --- | --- | --- | --- |
| Precision | $\frac{TP}{TP+FP}$ | 預測為正的樣本中，多少是真的正 | 預測準確性 |
| Recall    | $\frac{TP}{TP+FN}$ | 實際為正的樣本中，多少被找出 | 不漏掉真實正樣本 |
| F1-score  | $2 \times \frac{P \times R}{P + R}$ | Precision 與 Recall 的平衡 | 綜合指標 |
