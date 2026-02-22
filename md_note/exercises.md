## Exercise 4

### 🧩 **Question 4: Anti-monotonicity**

### 題目在問什麼：

題目假設現在我們只有 **一條資料序列**（不是一個序列資料庫）。
在這種情況下，**一個 pattern 序列的 support** 定義為：

> 它在資料序列中「出現的不同方式（distinct occurrences）」的個數。

而所謂的「不同方式」指的是：
存在兩個不同的映射函數 φ₁ 和 φ₂，且在某個位置 i 上有 φ₁(i) ≠ φ₂(i)。

題目問：

1. 在這樣的設定下，**支援度 (support)** 與**子序關係 (subsequence relation)** 之間是否仍具有**反單調性 (anti-monotonicity)**？
2. 你能否想到另一種「不同出現 (distinct occurrence)」的定義？在那個定義下，反單調性還成立嗎？

---

### 背景概念：

**反單調性 (Anti-monotonicity)** 是序列探勘裡很重要的性質。
意思是：

> 如果某個序列 `p` 不是頻繁的（support 太低），那麼它的所有超序列 (supersequence) 一定也不是頻繁的。

形式上：
[
p_1 \subseteq p_2 \Rightarrow support(p_1) \ge support(p_2)
]

這個性質對於**Apriori**、**GSP** 這類演算法至關重要。

---

### 詳解：

#### (1) 在這種「單一資料序列、distinct occurrence 計數」的情況下，反單調性還成立嗎？

👉 **不一定成立！**

**原因：**

* 反單調性要求：若 q 是 r 的子序列（q ⊆ r），則 `support(q)` ≥ `support(r)`。
* 但在這裡，support 是**不同嵌入方式的數量**。
* 有時候，當 pattern 變長時（例如加上一個元素），會導致「原本重複的嵌入」變得可以區分成新的 distinct occurrence。

🔹**舉例說明：**

假設資料序列：

```
S = a b a b
```

* Pattern q = `a`
  → 可以在位置 1、3 出現，共 2 次。
  → 所以 support(q) = 2。

* Pattern r = `a b`
  → 它的 distinct occurrences：
  (1) φ₁ = [1→a, 2→b]
  (2) φ₂ = [1→a, 4→b]
  (3) φ₃ = [3→a, 4→b]
  → support(r) = 3。

可以看到：

> support(a b) = 3 > support(a) = 2 ❌
> 這就違反了反單調性！

---

#### (2) 是否能想到另一種「distinct occurrence」定義，使反單調性恢復？

可以。
舉例來說：

**替代定義：**

> 我們把「distinct occurrence」定義為一組**不重疊 (non-overlapping)** 的出現。

例如：

* 若 `S = a b a b`，pattern `a b` 的不重疊出現是：

  * (a₁,b₂)
  * (a₃,b₄)
    → support = 2。

  而 pattern `a` 的不重疊出現也是：

  * (a₁)、(a₃)
    → support = 2。

  所以這裡 `support(a b) ≤ support(a)` ✅
  → **反單調性成立。**

---

✅ **結論：**

| 定義方式                 | 反單調性成立？ | 說明                   |
| -------------------- | ------- | -------------------- |
| 按照「不同映射 φ」計 distinct | ❌ 否     | pattern 變長可能產生更多不同映射 |
| 按「不重疊出現」計 distinct   | ✅ 是     | 超序列的出現不可能比子序列更多      |

---

### 🧩 **Question 5: Transitivity of the subsequence relation**

### 題目在問什麼：

這題要你證明「為什麼子序關係的**遞移性 (transitivity)**」可以用來保證反單調性。

---

### 背景說明：

在序列探勘裡，我們用子序關係 (⊆) 來定義 pattern 包含關係。
反單調性說的是：

> 若 q ⊆ r，則 support(q) ≥ support(r)。

要保證這一點成立，我們需要確保「⊆」這個關係是**遞移的**。
也就是：
[
q ⊆ r \text{ 且 } r ⊆ s \Rightarrow q ⊆ s
]

---

### 詳解：

#### (1) 為什麼「遞移性」能保證反單調性？

因為：

* 若一個超序列 s 含有 r 的出現，那每個 r 的出現都包含一些 q 的出現（若 q ⊆ r）。
* 因此 s 的所有 r 出現對應到 q 的出現。
* 所以 q 的支援度至少與 r 一樣大。
* 如果子序關係不是遞移的，那就無法保證這種包含邏輯成立。

---

#### (2) 證明 q ⊆ s

給定：

* q = q₁ q₂ … q_k
* r = r₁ r₂ … r_m
* s = s₁ s₂ … s_n
  並且：

q ⊆ r ⇒ ∃ φ₁: [1,k]→[1,m]
r ⊆ s ⇒ ∃ φ₂: [1,m]→[1,n]
使得：

* q[i] = r[φ₁(i)]
* r[j] = s[φ₂(j)]
* 且 i<j ⇒ φ₁(i)<φ₁(j)，φ₂(i)<φ₂(j)

那我們可以構造組合映射：
[
φ = φ₂ ∘ φ₁
]
（即 φ(i) = φ₂(φ₁(i))）

那麼：

* q[i] = r[φ₁(i)] = s[φ₂(φ₁(i))] = s[φ(i)]
* 且若 i<j ⇒ φ₁(i)<φ₁(j) ⇒ φ₂(φ₁(i)) < φ₂(φ₁(j))

因此 φ 滿足 subsequence 的條件。
⇒ **q ⊆ s。**

✅ 得證遞移性。

---

### 🧩 **Question 6: Variations on a theme**

### 題目在問什麼：

這題是把前面「字母序列」改成「**項目集合序列 (itemset sequence)**」。

例如，一個顧客的購物紀錄可能是：

```
({milk, bread}) ({milk, eggs}) ({bread})
```

要你：

1. 給出**子序關係**（subsequence relation）的定義。
2. 給出**支援度 (support)** 的定義。
3. 驗證在這種情況下，支援度與子序關係仍具有**反單調性**。

---

### 詳解：

#### (1) 子序關係的定義

給兩個 itemset sequence：

* Pattern: P = (X₁, X₂, …, X_k)
* Data sequence: S = (Y₁, Y₂, …, Y_m)

定義：
[
P ⊆ S \iff ∃ 1 ≤ i₁ < i₂ < … < i_k ≤ m \text{ 使得 } X_j ⊆ Y_{i_j} \text{ 對所有 } j
]

也就是：

* Pattern 的每一個項目集合 X_j 必須能被嵌入到 S 的某個時間點 Y_{i_j}。
* 且這些時間點必須保持順序。

---

#### (2) 支援度定義

給一個資料庫 D = {S₁, S₂, …, S_N}
則：
[
support(P) = |{ S_i ∈ D \mid P ⊆ S_i }|
]

即：有多少個顧客的購物序列含有此 pattern。

---

#### (3) 驗證反單調性

若 Q ⊆ P，則對任何資料序列 S：

* 若 S 支援 P（即 P ⊆ S）
* 由 subsequence 的定義可得 Q ⊆ S（因為 Q 的項目集合都包含於 P 對應的位置）

所以：
[
support(Q) ≥ support(P)
]

✅ 因此反單調性成立。

---

## Exercise 5

### Question 6

#### 🔹題目背景

在一個 **Bayesian Network (BN)** 中，每個節點 ( X_i ) 都有一個條件機率分佈：
[
p(X_i | X_{\text{pa}(i)})
]
其中 (\text{pa}(i)) 是該節點的父節點集合（parents）。

對於 **k 個節點** 的 BN，整體資料的對數似然函數（log-likelihood）是：
[
L = \sum_{i=1}^k \sum_{x_{\text{pa}(i)}} \Big[ n(x_i=0, x_{\text{pa}(i)}) \log p(x_i=0|x_{\text{pa}(i)}) + n(x_i=1, x_{\text{pa}(i)}) \log(1 - p(x_i=0|x_{\text{pa}(i)})) \Big]
]

---

#### 🔹問題 (a)：求偏導數

我們要對其中一個參數
[
p(x_j=0 | x_{\text{pa}(j)})
]
取偏導（partial derivative），也就是：
[
\frac{\partial L}{\partial p(x_j=0 | x_{\text{pa}(j)})}
]

---

##### ✳️ 先簡化符號

令：

* (n_0 = n(x_j = 0, x_{\text{pa}(j)}))
* (n_1 = n(x_j = 1, x_{\text{pa}(j)}))
* (p_0 = p(x_j = 0 | x_{\text{pa}(j)}))

那麼對應的部分 log-likelihood 就是：
[
L_j = n_0 \log p_0 + n_1 \log(1 - p_0)
]

（整體 L 是對所有節點 i 加總，但其他節點 i≠j 的部分與 p₀ 無關，所以我們只關注這個項）

---

##### ✳️ 對 p₀ 取偏導：

[
\frac{\partial L_j}{\partial p_0} = \frac{n_0}{p_0} - \frac{n_1}{1 - p_0}
]

這就是題目中提到的：
[
\frac{\partial L}{\partial p(x_j = 0 | x_{\text{pa}(j)})}
= \frac{n(x_j = 0, x_{\text{pa}(j)})}{p(x_j = 0 | x_{\text{pa}(j)})}

* \frac{n(x_j = 1, x_{\text{pa}(j)})}{1 - p(x_j = 0 | x_{\text{pa}(j)})}
  ]

---

##### ✳️ 檢查依賴性

注意這個偏導式中：

* 出現的 (n_0, n_1) 是**資料中的已知數據**；
* 唯一未知的參數是 (p_0)。

✅ 所以，這證明了「偏導只依賴該參數本身」，不依賴其他未知的參數。

---

#### 🔹問題 (b)：設偏導為 0，解出 MLE

取：
[
\frac{\partial L_j}{\partial p_0} = 0
]

代入：
[
\frac{n_0}{p_0} - \frac{n_1}{1 - p_0} = 0
]

---

##### ✳️ 解這個方程：

乘開分母：
[
n_0(1 - p_0) - n_1 p_0 = 0
]
[
n_0 - n_0p_0 - n_1p_0 = 0
]
[
n_0 = p_0 (n_0 + n_1)
]

所以：
[
p_0 = \frac{n_0}{n_0 + n_1}
]

---

##### ✳️ 回代符號：

[
p(x_j = 0 | x_{\text{pa}(j)}) = \frac{n(x_j = 0, x_{\text{pa}(j)})}{n(x_j = 0, x_{\text{pa}(j)}) + n(x_j = 1, x_{\text{pa}(j)})}
]

而分母 (n(x_j = 0, x_{\text{pa}(j)}) + n(x_j = 1, x_{\text{pa}(j)}) = n(x_{\text{pa}(j)}))，
所以也可以寫成：

[
p(x_j = 0 | x_{\text{pa}(j)}) = \frac{n(x_j = 0, x_{\text{pa}(j)})}{n(x_{\text{pa}(j)})}
]

---

#### 🔹這代表什麼？

這個結果就是：

> **最大概似估計（MLE）對應於觀察到的條件相對頻率。**

換句話說，對任一節點 (X_j)：
[
\hat{p}(X_j = 0 | X_{\text{pa}(j)} = x_{\text{pa}(j)}) = \frac{\text{「出現次數」}}{\text{「該 parent 組合的總次數」}}
]

這是很自然的結果——在貝氏網路中，每個條件機率表（CPT）都可以用觀察次數比例來估計。

---

#### 🔹總結重點（中文總覽）

| 步驟  | 內容                | 結果                                                          |
| --- | ----------------- | ----------------------------------------------------------- |
| (1) | 寫出 log-likelihood | (L_j = n_0\log p_0 + n_1\log(1-p_0))                        |
| (2) | 對 (p_0) 取偏導       | (\frac{∂L_j}{∂p_0} = \frac{n_0}{p_0} - \frac{n_1}{1 - p_0}) |
| (3) | 設偏導 = 0           | (\frac{n_0}{p_0} = \frac{n_1}{1-p_0})                       |
| (4) | 解出 MLE            | (p_0 = \frac{n_0}{n_0 + n_1})                               |
| (5) | 解釋                | 機率 = 「該情況發生的次數」 ÷ 「該父組合的總次數」                                |

---

如果你願意，我可以幫你畫一個簡圖：

* 節點 (X_j)
* 父節點 (X_{\text{pa}(j)})
* 怎麼用資料表中的 counts (n(x_j, x_{\text{pa}(j)})) 來估計這個條件機率表（CPT）

要我畫這個圖示化的版本嗎？
