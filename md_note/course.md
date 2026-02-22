## (OK) 37A 37B Classification Trees
### (OK) (37A P.32) How many possible binary split for a categorical attribute?
$2^{L - 1} - 1$
- L: distinct values  

因為一個集合可能的非空＋非全部subsets有$2^{L-2}$個，會兩兩對應（補集complement），因此要除以2，等於$2^{L - 1} - 1$。

### (OK) (37A P.33,34) How many optimal binary split for a categorical attribute?  
exc1- question1 - b  
算出所有categories出現某一個class的機率，排序這個機率，只從排序的中間切斷他們。  

Example:
1. 先算conditional probability 對 class 0
    P(0|x=a), P(0|x=b), P(0|x=c), P(0|x=d)
2. 根據機率排序
    c<b<a<d
3. 中間的斷點就是optimal binary split
    {c},{c,b},{c,b,a}


### (OK) (37A P.31) How many possiable binary split for a numeric attributes?  
exc1 - question1 - c  
Ans: $n-1$  
若有$n$個不同的數值，可能的切點就是$n-1$個。

### (OK) (37A P.35~37) How many optimal binary split for a numeric attributes?
exc1 - question1 - d  
先把每個數值排序，計算這個數值在某個class的conditionsl probability (P(A|x=28), P(A|x=31)...)，然後看在數值的排序底下，probability被切成幾段。（有可能連續好幾個數值的機率是一樣的，那中間就不需要切點）  

Example:
|          |  8  |  12 |  14 |  16 |  18 |  20 |
| :------- | :-: | :-: | :-: | :-: | :-: | :-: |
| **P(A)** | 0.5 | 0.5 | 1.0 | 1.0 | 1.0 | 0.5 |
| **P(B)** | 0.5 | 0.5 | 0.0 | 0.0 | 0.0 | 0.5 |

### (OK) Gini-Index and Impurity Reduction
exc1 - question1 - e  

Gini-Index 用來衡量一個節點的混亂程度。若一個節點完全屬於同一個類別，則gini-index為0。

- Gini-Index formula: 
    $$
    i(t) = P(0 \mid t) , P(1 \mid t)
    $$
    或等價地寫成：
    $$
    i(t) = P(0 \mid t) [1 - P(0 \mid t)]
    $$
    * $P(0 \mid t)$：節點 $t$ 中屬於類別 0 的機率
    * $P(1 \mid t)$：節點 $t$ 中屬於類別 1 的機率

    **General Formula:**
    $$
    I(t) = 1 - \sum_{j=1}^{k} [P(j \mid t)]^2
    $$
    * $I(t)$：節點 (t) 的 Gini 不純度
    * $k$：類別（class）的總數
    * $P(j \mid t)$：在節點 (t) 中，屬於類別 (j) 的樣本比例

- Impurity Reduction （不純度降低）:  
    當一個節點 ( t ) 被分裂成左子節點 ( \ell ) 和右子節點 ( r ) 時，我們可以計算這次分裂後「不純度降低了多少」。這個值可以用來判斷「分裂是否有效」。
    $$
    \Delta i(S, t) = i(t) - [\pi(\ell) i(\ell) + \pi(r) i(r)]
    $$
    * $i(t)$：母節點（分裂前）的 Gini 不純度
    * $i(\ell), i(r)$：左右子節點的 Gini 不純度
    * $\pi(\ell), \pi(r)$：左右子節點樣本數佔整體樣本的比例

計算每種不同的 split 的 impurity reduction，以找出最好的 split 方法，步驟如下：
1. 畫出還沒 split 之前的 node，node裡面寫出不同class的比例，接著再向下左右分出兩個 nodes，node裡面一樣寫該node所有觀察值不同class的比例，向下分支的兩條線要寫分過去的觀察值的比例。
2. 計算每一個nodes的gini-index。
3. 利用以上所有的資訊計算impurity reduction。
4. 找出最大的impurity reduction，該split就是最好的分法。

### (OK) (37B P.13,17) Total **cost of a tree** & **Resubstitution error**
exc1 - question3
- Formula
    $$
    C_{\alpha}(T) = R(T) + \alpha |\tilde{T}|
    $$

- 總成本的分解 — Decomposition of Total Cost
    $$
    C_{\alpha}(T) = R(T) + \alpha |\tilde{T}| = \sum_{t \in \tilde{T}} \big[ R(t) + \alpha \big]
    $$

- Resubstitution error
$$
R(T) = \frac{\text{Number of wrong classifications made by } T}{\text{Number of examples in the training sample}}
$$

- 對單一終端節點的R(t)：$\frac{\text{那一個節點自己的錯誤分類數量}}{\text{整棵樹的樣本數量}}$

* $R(T)$：衡量模型「**有多準確**」
* $|\tilde{T}|$：衡量模型「**有多複雜**」
* $\alpha$：調整這兩者的權重
* $C_{\alpha}(T)$：綜合考慮誤差與複雜度的「**總成本**」，剪枝時要找到讓它最小的樹。
* $t$：單一終端節點（leaf node）



**公式的作用與直覺說明：**
這個公式的目的是要在「**模型的準確率**」與「**模型的簡單性**」之間找到平衡。

* 第一項 $R(T)$：越小表示越準確
* 第二項 $\alpha |\tilde{T}|$：越小表示模型越簡單
* 因此：

* 若 $\alpha = 0$：只重視準確率 → 模型容易**過擬合（overfitting）**
* 若 $\alpha$ 很大：重懲複雜度 → 模型會**被強迫變簡單（underfitting）**

👉 最佳的剪枝就是找到能最小化 $C_{\alpha}(T)$ 的那棵樹。













### (37B P.22) (OK) Cirtical alpha value for pruning
- 步驟
    1. 計算每個不是leaf node 的 critical alpha value
    2. 找到最小的那一個，並且對該 node 做 prune
    3. 得到一個新的 prune 過的樹，重複步驟1, 2直到剩下 root node

- g(t) 是什麼
g(t) 是節點 t 的 α 臨界值，也就是說，如果 α 值大於 g(t) 那麼剪掉這個子數會比較划算（cost 比較小）。它衡量了**每刪一個葉子會增加多少誤差**。  
 $$
g(t) = \frac{\text{剪掉子樹造成的誤差增加}}{\text{剪掉子樹節省的葉子數}}
$$



- 公式
$$
g(t) = \frac{R(t) - R(T_t)}{|\tilde{T_t}| - 1}
$$

|       符號      | 名稱   | 意思                 |   
| :-----------: | ------- | --------- | 
|      $t$ | 非終端節點（non-terminal node）    | 我們正在考慮是否要剪掉的那個節點。  |         
|     $T_t$     | 以節點 (t) 為根的**子樹（subtree）**      | 包含節點 (t) 及其所有後代節點。 |          
|    $R(T_t)$   | 子樹 (T_t) 的整體訓練誤差（resubstitution error） | 沒有被剪枝前的誤差。    |            
|     $R(t)$    | 若將整個子樹 (T_t) 剪掉、改成一個單一葉節點 (t) 時的誤差  | 剪枝後該節點的誤差。   | 



### <mark>(37B P.15) (OK) What is **smallest minizing subtree**</mark>
exc1 - question3 - e

給定一個 value α ，在這個值底下cost最小的剪枝樹，可能有多個相同最小cost的剪枝樹，要找最小的那一顆。

> **Definition (Smallest Minimizing Subtree)**
> For any value of $\alpha$, there exists a smallest minimizing subtree $T(\alpha)$ of $T_{\max}$ that satisfies:
>
> 1. $T(\alpha)$ minimizes the total cost for that value of $\alpha$:
>    $$
>    C_{\alpha}(T(\alpha)) = \min_{T \leq T_{\max}} C_{\alpha}(T)
>    $$
>
> 2. $T(\alpha)$ is a pruned subtree of all trees that minimize the total cost:
>    $$
>    \text{if } C_{\alpha}(T) = C_{\alpha}(T(\alpha)) \text{ then } T(\alpha) \leq T
>    $$
>
> Here, $T' \leq T$ means that $T'$ is a **pruned subtree** of $T$.

* **最大樹 (T_{\max})**：是從訓練資料生成的**完整決策樹**（未剪枝、最複雜的樹）。
* **(T(\alpha))**：是對應某個懲罰參數 (\alpha) 下，能**最小化成本函數 (C_{\alpha}(T))** 的樹。
* 「**smallest minimizing subtree**」意思是：
  在所有能達到最小成本的樹中，選出**最小的一棵**（也就是被剪得最多的那一棵）。

## <mark>**38A** Tree ensembles: Bagging, Boosting and Random Forests</mark>

## (OK) 38B 39A Text Clessification - Naive Bayes & Logistic Regression

### (OK) (38B P.12~20) Text Classification - Multinomial naive bayes
1. Find the Vocabulary
    找出所有在 **training documents** 中出現過的 **不重複單字**。
    這個集合稱為 **vocabulary**：
    $$
    V = { w_1, w_2, \dots, w_{|V|} }
    $$

2. 計算三個數值
    1. **Vocabulary 大小**
    $$
    |V| = \text{number of unique words in all training documents}
    $$

    2. **Class 1 的所有 documents 的字數（含重複）**
    $$
    N_1 = \sum_{d \in \text{class 1}} \text{count(all words in } d)
    $$

    3. **Class 2 的所有 documents 的字數（含重複）**
    $$
    N_2 = \sum_{d \in \text{class 2}} \text{count(all words in } d)
    $$

3. Laplace Smoothing 機率估計公式
    對於每個單字 $w_i$ 和每個類別 $c$，使用 **Laplace smoothing** 的機率估計公式如下：

    $$
    \hat{P}(w_i \mid c) =
    \frac{\text{count}(w_i, c) + 1}
    {\sum_{w_j \in V} \text{count}(w_j, c) + |V|}
    $$
    * $\text{count}(w_i, c)$：單字 (w_i) 在所有屬於類別 (c) 的文件中出現的次數
    * $|V|$：vocabulary 的大小
    * 分母的 $\sum_{w_j \in V} \text{count}(w_j, c)$：類別 (c) 的所有字詞總數
    * 加上 (+1) 與 (+|V|) 是 **Laplace smoothing**，避免機率為 0
    
    文字敘述是：
    $$
    \hat{P}(w_i \mid c) =
    \frac{(單字W_i在所屬類別出現的次數) + 1}
    {類別C的所有字詞總數 + vocabulary 的大小}
    $$

    Note: General 的機率估計公式
    $$
    P(w \mid c) = \frac{N_{w,c} + \alpha}{N_c + \alpha |V|}
    $$
    $\alpha = 1$ 時就是使用Laplace smoothing



4. 估計 Class Prior Probabilities  
    文字敘述：
    $$
    \hat{P}(c) =
    \frac{這個class的文件總數}
    {所有文件總數}
    $$
    公式：
    $$
    \hat{P}(c) =
    \frac{N_c}{N_{\text{doc}}}
    $$
    * $N_c$：屬於類別 $c$ 的文件數量
    * $N_{\text{doc}}$：所有文件的總數

5. 給定一個新的document，預測他對某個class的機率
    會有兩個步驟，第一個步驟先計算對新文件的後驗分數，第二個步驟利用前面的結果算出正規化後驗機率
    1. Multinomial Naive Bayes 的未正規化分數（Posterior 未正規化）
        $$
        \text{score}(c \mid d) \propto P(c) \prod_{w \in V} [P(w \mid c)]^{tf(w, d)}
        $$
        * $c$：類別（class）
        * $d$：測試文件集合（document）
        * $V$：詞彙表（vocabulary）
        * $w$：單字（word）
        * $tf(w, d)$：單字 $w$ 在文件 $d$ 中的出現次數  
        
        文字敘述是：
        $$
        \text{score}(c \mid d) \propto \text{(這個 class 在 training document 出現的機率)}\prod_{新文件的每一個單字}\text{(該單字在training資料裡面此類別的機率估計)}
        $$


    2. 正規化後的後驗機率（Normalized Posterior Probability）
        $$
        \hat{P}(c \mid d) =
        \frac{\text{score}(c \mid d)}
        {\sum_{c'} \text{score}(c' \mid d)}
        $$
        文字敘述是：
        $$
        \hat{P}(c \mid d) =
        \frac{\text(testing文件對我們感興趣的class的分數)}
        {\sum_{所有class}\text{(testing文件對該class的分數)}}
        $$

### (OK) Logistic Regression
參考 exc2 - problem3
1. 模型概念

    Logistic Regression 是一種**線性分類模型**，用來預測某事件發生的機率。
    模型假設輸入變數與對數勝率（log-odds）之間是**線性關係**：

    $$
    \text{logit}(P(Y=1)) =
    \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \cdots + \beta_k x_k
    $$

    轉換成機率形式：

    $$
    P(Y=1) =
    \frac{\exp(\beta_0 + \beta_1 x_1 + \beta_2 x_2 + \cdots + \beta_k x_k)}
    {1 + \exp(\beta_0 + \beta_1 x_1 + \beta_2 x_2 + \cdots + \beta_k x_k)}
    $$

2. 模型中的參數意義
    * **$\beta_0$**（Intercept）  
    表示當所有變數為 0 時，事件發生的基本機率（或 baseline odds）。  
    在特定應用中，它常代表某種「固定優勢」或「基準情況」。

    * **$\beta_i$**（Coefficient for $x_i$）  
    表示變數 $x_i$ 對 log-odds 的影響程度。  
    若 $\beta_i > 0$，則 $x_i$ 增加會提高事件發生的機率；  
    若 $\beta_i < 0$，則會降低事件發生的機率。

3. 解題的**比較與判斷**
   * 若 (P > 0.5)：預測屬於正類（例如「贏」）。
   * 若 (P < 0.5)：預測屬於負類（例如「輸」）。

4. 線性分類規則（Linear Classification Rule）
    因為 logistic function 在 (P=0.5) 時，(z=0)，
    所以分類邊界為：

    $$
    \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \cdots + \beta_k x_k = 0
    $$

    對應的分類規則：

    $$
    \begin{cases}
    \text{Predict } Y=1, & \text{if } \beta_0 + \sum_i \beta_i x_i > 0 \
    \text{Predict } Y=0, & \text{if } \beta_0 + \sum_i \beta_i x_i < 0
    \end{cases}
    $$

5. 係數合理性檢查（Sign Interpretation）

    檢查每個 (\beta_i) 的符號是否符合常理：

    * 若變數應該增加勝率 → $\beta_i > 0$
    * 若變數應該降低勝率 → $\beta_i < 0$

### Note
- exp(x) = $e^x$ (exc2 - question3)


## (OK) 39B 40A 40B Undirected Graphs
### (OK) (39B P.35~40) What is U-terms? 
1. 背景：從 Joint Distribution 開始  
    在 **無向圖模型 (Markov Random Field, MRF)** 或 **log-linear model** 裡，我們通常把聯合機率 $P(X_1, X_2, X_3)$ 寫成指數形式（log-linear form）：
    $$
    \log P(x_1, x_2, x_3) = u_0 + u_1 x_1 + u_2 x_2 + u_3 x_3 + u_{12} x_1x_2 + u_{13} x_1x_3 + u_{23} x_2x_3 + u_{123} x_1x_2x_3
    $$
    這些 $u$-terms 就是你看到的 **u-terms**。

2. 每個 $u$-term 的含義
    | 符號                         | 名稱                 | 代表的關係                    |
    | :------------------------- | :----------------- | :----------------------- |
    | $u_1, u_2, u_3$          | main effects       | 各變數單獨對 log 機率的影響         |
    | $u_{123}$                | 3-way interaction  | 三變數之間的共同交互作用（三角形 clique） |

    所以在無向圖中，**一個非零的 $u_{ij}$** 代表節點 $i$ 和 $j$ 之間存在一條邊。**如果 $u_{ij} = 0$**，就代表這兩個變數之間在圖中是沒有直接連線的（可能 conditionally independent）。

3. 如何從一個 undirected graphs 找到 u-terms  
空集合 + （所有單一的nodes - 這個其實已經包含在所有cliques的subsets裡面了） + 所有的cliques以及他們的所有非空subsets。  

（我找不到在課本的哪裡）

### (OK) (40A40B P.8,11)Fitted count
HW2 - question 2
exc3 - question 3
| 模型類型 | 假設 | Fitted probability | Fitted count |
| ------- | ----- | ------- | --------- |
| **Saturated model** | 無限制 | $\displaystyle \hat{P}(G,E)=\frac{n(G,E)}{n}$ | $\displaystyle \hat{n}(G,E)=n(G,E)$ |
| **Independence model** | $G \perp E$ | $\displaystyle \hat{P}(G,E)=\frac{n(G)}{n}\cdot\frac{n(E)}{n}$ | $\displaystyle \hat{n}(G,E)=\frac{n(G),n(E)}{n}$ |


### (OK) (40A40B P.39) What is **running intersection properity (RIP)** and its **maximum likelihood fitted counts**? 
1. 先找出所有的cliques
2. 我們需要幫這些cliques排序
3. 排序方法是：前面所有的cliques union，再跟現在這個clique interset 產生的set（這個set就是這個clique的seperator），要是前面任意至少一個clique的subset。

要找得到這個order才是RIP(Running Intersection Properity)。

An ordering C1, . . . , Cm of the cliques of the graph has the running intersection property (RIP) iff:
$$
p.39 
$$
for some i < j, and for all j = 2, . . . , m.

We define the corresponding separator sets
$$
p.39
$$
with S1 = null.

所有的cliques放分母，seperator放分子，就是這個圖的maximum likelihood fitted counts。

參考投影片42頁。

### (OK) (40A40B P.43~53) What is general formula for **Iterative Proportional Fitting (IPF)**?  
exc 3 - problem - c

1. IPF 是什麼？  
Iterative Proportional Fitting (IPF) 是一個用來**調整聯合機率表（joint probability table**的方法，目的是讓模型在所有 clique 的邊際分布（margins） 上符合觀察資料（Observed Margins）。  
換句話說：IPF 會反覆調整機率表的值，直到「對應於 cliques 的邊際」都等於觀察資料的邊際。  
這樣的最終分布就是滿足圖形獨立結構的 最大概似估計（MLE）分布。

2. IPF 公式  
    假設目前我們在調整的 clique 是 (C)（例如 (C = {A, B})）。

    則 IPF 的更新通式是：

    $$
    \hat{n}^{(t+1)}(x)
    =

    \hat{n}^{(t)}(x)
    \times
    \frac{n_{\text{obs}}(x_C)}{\hat{n}^{(t)}(x_C)}
    $$

    | 符號                                               | 意義                                                |
    | ------------------------------------------------ | ------------------------------------------------- |
    | $x$                                              | 一個完整的組合   |
    | $\hat{n}^{(t)}(x)$                               | 第 (t) 次迭代時，該組合的 fitted count                      |
    | $C$                                              | 目前要調整的 clique（例如 {Gender, Eye}）                   |
    | $x_C$                                            | 該組合中屬於 clique C 的部分（例如 (Gender=female, Eye=blue)） |
    | $n_{\text{obs}}(x_C)$                            | 在觀察資料中，clique C 的邊際計數（即這個組合的總數）                   |
    | $\hat{n}^{(t)}(x_C)$                             | 目前 fitted counts 的邊際（從所有 cell 加總得到）               |
    | $\frac{n_{\text{obs}}(x_C)}{\hat{n}^{(t)}(x_C)}$ | 調整比例（讓新的 fitted margin 匹配觀察到的 margin）             |









### (OK) (40A40B P.58~59)How to derive the formula of the **deviance of the fitted model**? 
對一個模型 ( M )，其對數似然為：
$$
L_M = \sum_x n(x) \log \hat{P}_M(x)
$$

而對於 **飽和模型 (saturated model)**（即沒有任何限制的模型）：
$$
\hat{P}*{sat}(x) = \frac{n(x)}{N}
$$
因此它的對數似然為：
$$
L*{sat} = \sum_x n(x) \log \frac{n(x)}{N}
$$

模型 ( M ) 的 deviance 定義為：
$$
\text{dev}(M) = 2 \times (L_{sat} - L_M)
$$

代入上面兩個對數似然：
$$
\text{dev}(M)
= 2 \sum_x \Big[ n(x) \log \frac{n(x)}{N} - n(x) \log \hat{P}_M(x) \Big]
$$

$$
\text{dev}(M)
= 2 \sum_x n(x) \log \frac{n(x)}{\hat{n}(x)}
$$


$$
\boxed{
\text{dev}(M) = 2 \sum_{x} n(x) \log \frac{n(x)}{\hat{n}(x)}
}
$$

| 符號                              | 意義                        |
| ------------------------------- | ------------------------- |
| $n(x)$| 觀察到的次數（observed count）    |
| $\hat{n}(x) = N \hat{P}_M(x)$| 模型預測的次數（fitted count）     |
| $N = \sum_x n(x)$| 總樣本數                      |
| $L_M$| 模型 ( M ) 的 log-likelihood |
| $L_{sat}$| 飽和模型的 log-likelihood      |
| $\text{dev}(M)$| 模型偏差（衡量模型與資料的差距）          |

> **“Deviance = 2 × Σ (observed × log(observed / fitted))”**

也就是：

$$
\boxed{
\text{dev}(M) = 2 \sum_{\text{cells}} \text{observed} \times \log \frac{\text{observed}}{\text{fitted}}
}
$$

參考 exc 3 - problem 5 - c

### (OK) (40A40B P.60) **Deivance difference**
假設：
$$
M_0 \subseteq M_1
$$
也就是說：

> ( M_0 ) 是 **較簡單的模型**（多一些限制），  
> ( M_1 ) 是 **較複雜的模型**（自由度更多）。  
> $M_0$, $M_1$ 是基於同一個觀察資料，只是用不同的結構擬和。

$$
\boxed{
\text{Deviance difference} = \text{dev}(M_0) - \text{dev}(M_1)
}
$$

由於：
$$
\text{dev}(M) = 2(L_{\text{sat}} - L_M)
$$

所以：
$$
\text{dev}(M_0) - \text{dev}(M_1)
= 2(L_{M_1} - L_{M_0})
$$


- **最常見的表達式**

$$
\boxed{
\Delta \text{dev} = 2 (L_{M_1} - L_{M_0})
}
$$

- **統計檢定意義**

當樣本數 ( N ) 很大時（大樣本近似）：

$$
2 (L_{M_1} - L_{M_0}) ;\approx; \chi^2_{\nu}
$$

其中：

| 符號 | 意義 |
| --- | --- |
| $\chi^2_{\nu}$ | 卡方分布 (chi-square distribution) |
| $\nu$ | $M_0$ 與 $M_1$ 之間的自由度差（或「限制數」） |
| $\nu$ = number of additional restrictions in $M_0$ compared to $M_1$ |


> **Deviance difference = 2 × (log-likelihood gain of larger model)**
>
> 用來檢驗：增加參數後（從 $M_0$ → $M_1$）是否顯著提升模型擬合。

$$
\boxed{
\text{dev}(M_0) - \text{dev}(M_1) = 2(L_{M_1} - L_{M_0}) \sim \chi^2_{\nu}
}
$$


### (OK) Factorisation for conditional indepent model  
exc 3 - question 3 - b

In general, X is independent of Y given Z if and only if
$$
P(x,y|z) = P(x|z)P(y|z)
$$
for all values xof X, y of Y, and for all values z of Z with P(Z= z) >0 (otherwise the conditional probability is not defined). 

1. P(X,Y |Z) = P(X |Z)P(Y |Z)
2. P(X |Y,Z) = P(X |Z)

### (to study) Observed deviance > critical value 時要 reject model，為什麼？  
exc 3 - question 2 - d

1. 什麼是 **observed deviance**？  
模型的 deviance $\text{dev}(M)=2(L_{\text{sat}}-L_M)$ 測量「飽和模型（完美擬合）與你要檢驗的模型 (M) 之間的差距」。數值越大表示模型 (M) 無法很好地重現觀察到的資料（observed vs fitted 差距大）。

2. 為什麼把 deviance 當成檢定統計量？  
    當 $M_0$ 為「虛無假設（null hypothesis）」的模型，且 $M_1$ 為包含 $M_0$ 的較複雜模型（巢狀模型），依據 **似然比檢定(likelihood-ratio test)** 與 **Wilks' 定理**，在樣本數很大時
    $$
    2(L_{M_1}-L_{M_0})\quad\text{（也就是};\text{dev}(M_0)-\text{dev}(M_1)\text{）}
    $$
    近似服從卡方分配$(\chi^2_\nu)$，$\nu$ 為兩模型之間參數數目的差（或相當於約束數）。因此可以用卡方分布的臨界值或 p-value 做檢定。

3. 決策規則（直觀）
* 設顯著水準 $\alpha$（例如 0.05）。
* 找出卡方分布 $\chi^2_\nu$ 在 $\alpha$ 下的臨界值（critical value）。
* 若 **觀察到的 deviance > 臨界值**，表示在 $M_0$ 為真時出現這麼大的 deviance 的機率 $<\alpha$。也就是說：資料 **太不像** 在 $M_0$ 下會出現的情形 → **拒絕 $M_0$**。換句話說：大 deviance → 資料與 $M_0$ 差異顯著 → 拒絕 $M_0$。

### (OK) (40A40B P.70,71) Hill-Climbing search
1. pick an initial model, e.g. the empty graph
2. neighbors
    - add an edge
    - delete an edge
    (without creating a chordless cycle of length > 3)
3. if all neighbors have higher AIC, stop and return the current model.
4. otherwise move to the neighbor with lowest AIC and return to 2.


## (OK) 41A 41B Frequent pattern mining for **sets**, **sequences** and **trees**
### (OK) (41A P.8~11) What is **Apriori algorithm**, what is the meaning?
Apriori algorithm 可以找出所有 frequent itemsets。

1. Apriori algorithm 的實際操作
    給定好幾筆不同的transactions，每一筆transcations都是有不同的值(A, B, C, D, E)組成的set，並且再給定一個minimun support。

    從一個set裡面只有一筆資料開始，找每個set包含在幾的transactions裡面，這這就是他的support，如果這個support > minimun support，就帶表他是frequent。接著，再把所有frequent的sets做組合，變成一個set裡面有兩筆資料，以此類推。

    在找下個level的candiate時，要注意，如果組合出的set，他的subset有已經不是frequent的set，就直接刪除，不作為下一個level的candidate。

    summerize: 畫出各個level的 “candidate, support, frequent?” 表格。

2. Aprioir algorithm 是什麼？  
    主要用於從大型資料集中找出頻繁項集(frequent itemsets)，進而產生有意義的關聯規則(association rules)。（ex:{牛奶} → {麵包} 買牛奶的人常常也會買麵包)

    Apriori property:
        - 如果一個itemsets是頻繁的，那他所有subsets也一定是頻繁的。
        - 如果一個itemsets是不頻繁的，那他所有supersets也一定是不頻繁的。


### (OK) What is the meaning of **closed frequent itemsets**?
Closed Frequent Itemsets 的定義換句話說：
    1. 這個set是frequent。
    2. 再加上其他項目進去，出現的次輸就會變少。

也就是只保留真正代表資料分布，而不是被更大集合重複覆蓋的sets。

### (OK) (41A P.25) What is the meaning of **maximal frequent itemset**?
### (OK) (41A P.26,40~41) What is Apriori-close algorithm, what is the meaning?
Apriori-Close algorithm 可以找出所有 closed frequent itemsets。

1. 如何實作aprioir-close algorithm?  
    基本上操作的方法跟apriori algorithm差不多，但是在prune時，除了考慮support > minimum support的條件以外，還要額外考慮這個set的support是否分他的subset一樣，如果一樣的話，這個set就要被剪掉。
    
    這樣就找到他所有的generator，接著再透過這些generator找到他們的closure。

    找colsure的方法：找到所有包含某個generator的transactions，接著找這些transactions的最大交集。

    summerize: 第一次畫出 “candidate, support, generator?” 從第一個level開始的各個表格。接著透過所有的generator找closure和support，也就是畫一個“generator, closure, support”的表格。
    
2. Apriori-close algorithm 是什麼？
    Apriori-Close 是一種從交易資料中找出閉合頻繁項集(Closed Frequent Itemsets)的演算法。
An itemset I is maximal frequent if
    - I is frequent and
    - no proper superset of I is frequent

可以先用 Apriori algorithm 找出所有 frequent itemsets，或者用 Apriori-Close algorithm 找出所有 closed frequent itemsets，然後再從中挑出那些沒有任何頻繁超集的項集，就能得到 maximal frequent itemsets。


    Closed Frequent Itemsets 的定義：一個set是closed，如果沒有任何他的superset有相同的support。


### (OK) (41A P.18,23) What is the meaning of **confidence** and **lift**?
在**關聯規則（Association Rules）**中，`confidence`（信賴度）與 `lift`（提升度）是兩個非常重要的評估指標，用來判斷規則是否有意義。

1. Confidence（信賴度）
    如果我們有一條關聯規則：
    $$
    A \Rightarrow B
    $$

    那麼它的 **信賴度（confidence）** 定義為：
    $$
    \text{confidence}(A \Rightarrow B) = \frac{\text{support}(A \cup B)}{\text{support}(A)}
    $$

    在所有包含 (A) 的交易中，有多少比例同時也包含 (B)。

    👉 換句話說，信賴度代表：

    > 「當顧客買了 A 時，也會買 B 的機率」。

2. Lift（提升度）
    $$
    \text{lift}(X \Rightarrow Y) = \frac{P(Y|X)}{P(Y)} = \frac{P(X \cap Y)}{P(X)P(Y)}
    $$

    解釋：

    * $P(Y)$：在所有資料中，Y 出現的機率。
    * $P(Y|X)$：在已經知道 X 出現的條件下，Y 出現的機率。
    * 若 X、Y **獨立**，理論上 $P(Y|X) = P(Y)$。

    所以：

    * 若 $\text{lift} = 1$：X 與 Y 獨立（沒有關聯）；
    * 若 $\text{lift} > 1$：X 出現會讓 Y 更可能出現 → **正相關**；
    * 若 $\text{lift} < 1$：X 出現會讓 Y 變得不太可能出現 → **負相關**。


### (OK) (41B P.9~13) What is GSP algorithm?
GSP (Generalized Sequential Pattern algorithm) 用來在資料中找出經常在一起，而且有序列關係的項目序列 (sequential patterns)。

一樣從level 1 開始畫出所有levels的 “candidate, support, frequent?” 表格。

在一個level的所有candidate找出來後，要先確認他的support > minimun support。

在組下一個level的candidate時，要看目前sequence長度(n) - 1個值，兩個sequences的前 n-1 個值要一樣，接著再把最後一個值的所有可能合併。合併時一樣要考慮不要合併出subset已經不是frequent的值。

### (OK) (41B P.24,27) Induced subtree V.S. embedded subtree
- Induced subtree  
不允許跳過中間的節點，一定要，一定要保持父子關係。

    1. 標籤相同(label preservation)
    2. 保持左右訓續(orser preservation)
    3. 保持父子關係(parent-child preservation)

- Embadded subtree  
可以跳過中間的節點，只要祖先與後代的關係不變，就算是匹配。
    1. 標籤相同(label preservation)
    2. 保持左右訓續(orser preservation)
    3. 保持祖先-後代關係(ancestor-descendant preservation)

### (OK) (41B P.25) What is maching function?
假設我們有兩棵樹：

* **T**：pattern tree（模式樹，通常比較小）
* **D**：data tree（資料樹，通常比較大）

matching function 記作：

$$
\phi : V_T \to V_D
$$
> 將模式樹 T 中的每個節點 ( w_i ) **對應**（map）到資料樹 D 中的一個節點 ( v_j )。

最後會長得像這樣
φ(w1) = v1 φ(w2) = v2 φ(w3) = v3
w1, w2, w3 是T(pattern tree)的節點，v1, v2, v3 是對應到的 D(data tree)的節點。

### (OK) (41B) What is FREQT, right-most occorence list(RMO list)?
1. 什麼是FREQT(Frequent Tree Mining algorighm)  
目標從多個data trees中找出所有平凡出現的frequent subtrees。  

就像是 Poriori 找 frequent itemsets或是 GSP找frequent sequence一樣。  

FREQT要做的兩件事：
    1. 生成候選樹 (generate candidate trees)
    2. 幾算每顆候選數出現的次數

2. Right-Most Extension  
這是FREQT生成候選數的方式。  

在對T做depth-first preorder traversal後，節點一順序編號，找出從跟節點到最後一個節點的路徑（右端路徑），新的節點只能加在這條路徑上的節點之一的右端位置。

3. <mark>Right-Most Occurence List(RMO list)</mark>  
RMO 會記錄這個 pattern tree 的 right-most leaf在每一顆data tree中出現的所有位置。（一個pattern tree 可能會出現在 一個data tree 好幾次，會紀錄出現的每一次的ritht-most leaf）  

只紀錄每一個match的right most node，但是會間接包含整條right most path的資訊。因為透過right most node的位置就可以追溯出right most path 上所有節點在data tree 中的對應位置。

目的是為了讓下一次的搜尋變得更簡單，只要從有被記錄的那幾個位置往下搜尋就可以了。

RMO list 是為了induced subtree mining設計的，不適用於embedded subtrees。

請參考 exc4 - question3 - (h)

## (OK) 42A 42B Bayesian Networks
### (OK) What is **Bayesian Network** and **DAG** (Directed Acyclic Graph)
DAG (Directed Acyclic Graph) 是 有向無環圖。  
`Bayesian Network（貝葉斯網路） = 一個帶有機率意義的有向無環圖（DAG）`  
也就是說，DAG 是一個結構（Structure），描述哪些變量之間有「依賴關係」。  
Bayesian Network 則是在這個結構上，加入了機率分布（Parameters） 的模型。

### (OK) (42A P.14~22) **Construction of DAG** and **Factorization**
在construction of DAG的過程中，就可以看出DAG與他的faxtorization之間的關係。
1. <mark>Construction of DAG</mark>
根據變量之間的條件獨立關係，決定每個節點的父節點，並畫出對應的有向邊。
    1. 一開始先假設完全有相圖
    2. 利用條件獨立性刪邊，並且也同時對P(X)做調整。
    3. 最後產生一個DAG，以及一個P(X)公式。而這個P(X)公式就是這個DAG的factorization。
2. DAG 的 Factorization怎麼得出？  
    對於一個有 $k$ 個隨機變數的系統
    $$
    X = (X_1, X_2, \dots, X_k)
    $$
    以及對應的 **有向無環圖 (DAG)** 結構 $G$，
    其對應的機率分解（factorization）為：

    $$
    P(X_1, X_2, \dots, X_k) = \prod_{i=1}^{k} P(X_i \mid \text{pa}(X_i))
    $$

    |              符號              | 意思                                         |
    | :--------------------------: | :----------------------------------------- |
    |   $P(X_1, X_2, \dots, X_k)$  | 聯合機率分布（所有變數同時出現的機率）                        |
    |             $X_i$            | 第 $i$ 個隨機變數                                |
    |       $\text{pa}(X_i)$       | $X_i$ 的父節點（parents），即在 DAG 中指向 $X_i$ 的節點集合 |
    | $P(X_i \mid \text{pa}(X_i))$ | 在給定其父節點值時，$X_i$ 的條件機率                      |
    |       $\prod_{i=1}^{k}$      | 表示對所有節點的乘積（因為每個節點對應一個條件機率項）                |

    結論：給定一個 DAG，就可以唯一地寫出其 factorization 公式。反之，若給定 factorization 形式，也能推導出對應的 DAG。  

    簡單來說，每個node以他的parent(s)為條件的機率 相乘，就是一個DAG的factorization。

### (OK) (42A P.24~33) Moral graph
1. What is moral graph?  
給定一個DAG，他對應的moral graph 是一個無相圖，並且要根據以下的規則建構該無相圖：
    1. Marry the parents: 對於每個node，把他的所有parents兩兩相連。  
    2. Drop the directions: 將所有的有向邊變成無相邊。

2. 如何使用 moral graph 在 Bayesian Network 的進行條件獨立性分析？  
在bayesian network(DAG)中，要判斷兩個nodes是否獨立，理論上應該使用d-separation（方向性分離）規則，但是這個規則相對複雜。而另一個方法是把DAG轉成Moral Graph，然後用更簡單的graph separation（普通圖分離）來判斷獨立性。
    1. 先確定想知道的獨立性關係有哪些變量(nodes)
    2. 只考慮這些nodes以及他們的所有parents
    3. Moralize the DAG
    4. 進行簡單的graph separation 以檢查獨立性：刪除conditioned nodes以及連接他的邊，檢查欲判斷的nodes有沒有分離。

### ~~Why moral graph can check Independence Properties?~~

### (OK) (42B P.8) Scoring Functions AIC & BIC
1. 如何計算這兩個值，數學符號是什麼  
**AIC**（Akaike Information Criterion） 和 **BIC**（Bayesian Information Criterion）是用在 **貝葉斯網路結構學習（Structure Learning）** 的 **模型評分函數 (Scoring Functions)**。

    1. **AIC（Akaike Information Criterion）**
        $$
        \text{AIC}(M) = L_M - \text{dim}(M)
        $$
        或更常見的統計寫法（帶常數）：
        $$
        \text{AIC}(M) = 2L_M - 2 \text{dim}(M)
        $$
        在貝葉斯網路中常簡化掉常數 2（因為比較模型時不影響排名），所以可以寫成上面那樣。
        |         符號        | 名稱             | 含義                                 |
        | :---------------: | :------------- | :--------------------------------- |
        |       ( M )       | 模型             | 指一個貝葉斯網路結構                         |
        |      ( L_M )      | log-likelihood | 給定模型 ( M ) 下的對數似然值，反映模型對數據的擬合程度    |
        | ( \text{dim}(M) ) | 模型維度           | 模型中自由參數的個數 |
        | ( \text{AIC}(M) ) | AIC 分數         | 越大（或越小，取決於定義）表示模型越好，但要在擬合與簡潔之間取得平衡 |

    2. BIC（Bayesian Information Criterion）

        $$
        \text{BIC}(M) = L_M - \frac{log n}{2} \text{dim}(M) \
        $$

        |                 符號                 | 名稱                | 含義               |
        | :--------------------------------: | :---------------- | :--------------- |
        |                ( M )               | 模型                | 一個候選的貝葉斯網路結構     |
        |               ( L_M )              | log-likelihood    | 給定模型 ( M ) 的對數似然 |
        |          ( \text{dim}(M) )         | 模型維度              | 模型的參數數量          |
        |                ( n )               | 樣本數               | 用來估計模型的資料點數量     |
        | ( \frac{1}{2}\text{dim}(M)\log n ) | 懲罰項（penalty term） | 資料越多、參數越多，懲罰越重   |
        |          ( \text{BIC}(M) )         | BIC 分數            | 衡量模型的擬合與複雜度折衷    |

2. BIC Score 越高越好，為什麼？exc5-problem3-e-sol裡面有講到。

    1. log-likelihood $L_M$  
    $L_M$ 越大 → 模型擬合資料越好。

    2. 懲罰項 $- \text{dim}(M)$ 或 $- \frac{log n}{2} \text{dim}(M)$  
    懲罰越大 → 模型越複雜、要扣分。

    3. AIC / BIC 總和  
    $L_M - \text{penalty}$：想要 **擬合高、懲罰低** → 值越大越好。  
- 這頁投影片每一頁的符號代表什麼
- 為什麼第六頁可以簡化成最下面的公式


### (OK) (42B P.6,7) How to count maximum likelihood & log likelihood in scoring functions(AIC, BIC)?
Maximum likelihood 的計算 exercise 5 - question 3 - (a) 有例子可以參考。  
log-likelihood 的計算exercise 5 - question 3 - (b) 有例子可以參考。  

1. **Maximum Likelihood Estimation**

    |                  符號                  |       名稱       | 含義                                                              |
    | :----------------------------------: | :------------: | :-------------------------------------------------------------- |
    |                 $X_i$                |   第 $i$ 個隨機變量  | 貝葉斯網路中的節點之一                                                     |
    |            $\text{pa}(i)$            |      父節點集合     | 所有直接指向 $X_i$ 的變量集合                                              |
    |                 $x_i$                | 變量 $X_i$ 的具體取值 | 例如 $X_i = 0$ 或 $X_i = 1$                                        |
    |          $x_{\text{pa}(i)}$          |    父節點的取值組合    | 例如若 $\text{pa}(i)={X_1, X_2}$，則 $x_{\text{pa}(i)} = (x_1, x_2)$ |
    |      $n(x_i, x_{\text{pa}(i)})$      |     聯合出現次數     | 資料中滿足 $X_i = x_i$ 且 $X_{\text{pa}(i)} = x_{\text{pa}(i)}$ 的紀錄數  |
    |         $n(x_{\text{pa}(i)})$        |    父節點組合出現次數   | 資料中 $X_{\text{pa}(i)} = x_{\text{pa}(i)}$ 的紀錄數                  |
    | $\hat{p}(x_i \mid x_{\text{pa}(i)})$ |   條件機率的最大似然估計  | 以頻率估計的條件機率                                                      |

    ---

    若 $X_i$ **沒有父節點**（即 $\text{pa}(i) = \varnothing$），則：

    $$
    \hat{p}(x_i) = \frac{n(x_i)}{n}
    $$

    |    符號    | 含義                   |
    | :------: | :------------------- |
    | $n(x_i)$ | 資料中 $X_i = x_i$ 的紀錄數 |
    |    $n$   | 資料的總紀錄數              |

2. **Log-Likelihood Score**

    $$
    L = \sum_{i=1}^{k} \sum_{x_i,, x_{\text{pa}(i)}} n(x_i, x_{\text{pa}(i)}) \log \frac{n(x_i, x_{\text{pa}(i)})}{n(x_{\text{pa}(i)})}
    $$

    |                              符號                             |              名稱              | 含義                 |
    | :---------------------------------------------------------: | :--------------------------: | :----------------- |
    |                             $L$                             | Log-likelihood score（對數似然分數） | 衡量模型對資料的擬合程度（越大越好） |
    |                             $k$                             |             節點總數             | 貝葉斯網路中變量的數量        |
    |               $\sum_{x_i,, x_{\text{pa}(i)}}$               |         對所有可能的取值組合求和         | 每個節點及其父節點的所有值組合    |
    |                  $n(x_i, x_{\text{pa}(i)})$                 |             聯合次數             | 如上所述               |
    |                    $n(x_{\text{pa}(i)})$                    |            父節點出現次數           | 如上所述               |
    | $\log \frac{n(x_i, x_{\text{pa}(i)})}{n(x_{\text{pa}(i)})}$ |            對數條件機率            | 每個事件組合的對數機率貢獻值     |



### (OK) (42B P.35) How to count the dim(M) in scoring functions(AIC, BIC)?
solution of exercise 5 - question 3 - (C) 裡面有詳細的解釋：  

`Suppose a node has k different parent configurations (possible value assignments to its parents), and it can take on m different values itself. Then the number of parameters associated with that node is k(m−1) because you have to estimate k different conditional distributions, and each conditional distribution requires the estimation of m−1 probabilities.`  

也就是，這個node的所有parent可能的組合(k) 乘以 這個node可能的值減一(m-1)，就是這個mode的dimension。最後還要把所有的node的dimension相加。得到以下公式：

$$
\text{dim}(M) = \sum_{i=1}^{k} (d_i - 1) \prod_{X_j \in \text{pa}(X_i)} d_j
$$

| 符號 | 名稱 | 意思 |
| :-------: | :------ | :------ | 
|   $M$  | 模型  | 指這個貝葉斯網路模型  |   |   |
| $k$ | 變量數量 | 網路中節點（或隨機變量）的數目 |
| $X_i$ | 第 $i$ 個變量 | 網路中的一個隨機變量 |
| $\text{pa}(X_i)$ | 父節點集合 | 在 DAG 中直接指向 $X_i$ 的節點（即 $X_i$ 的父節點） |
| $d_i$ | 變量 $X_i$ 的取值數量 | 若 $X_i$ 是離散變量，則 $d_i = \| \text{Val}(X_i) \|$，例如二元變量 $d_i = 2$ |
| $\prod_{X_j \in \text{pa}(X_i)} d_j$ | 父節點組合的總數 | 所有父節點的取值可能組合數量 |
| $(d_i - 1)$ | 每個條件機率分布需估的自由參數數 | 因為機率總和為 1，所以對 ( d_i ) 個值只需要 ( d_i - 1 ) 個自由參數 |
| $\text{dim}(M)$ | 模型的維度或參數總數 | 貝葉斯網中需要估計的所有條件機率參數的數量總和 |


### (OK) (42B P.42) **Mutual independence model** (=empty graph)
Mutual independence model means empty graph.

### (OK) (42B P.44) Markov Equivalence & V-Structures
1. Markov Equivalence:  
 Two DAGs are Markov equivalent if and only if
    1. they have the same skeleton (same undirected graph when you drop the directions of all edges), and
    2. they have the same immoralities (v-structures).

2. 怎樣算是有相同的 v-structures  
    在一個有向無環圖（DAG）中，一個 **v-structure**（也稱 **collider 結構**）是這樣的一種局部形狀：
    $
    A \to C \leftarrow B
    $
    而且 **A 和 B 之間沒有任何邊相連（不論方向）**。


    1. 舉例：以下是v-structure
        ```
        A → C ← B
        ```
        這是 v-structure，因為：

        * A 和 B 都指向同一個節點 C；
        * A 和 B 之間 **沒有邊**。


    2. 舉例：以下*不*是v-structure
        ```
        A → C → B
        ```
        ```
        A ← C ← B
        ```
        ```
        A → C ← B，但 A ↔ B 有邊
        ```
        （因為 A 和 B 之間有連結，所以不算「v」結構）

3. 判斷兩個圖是否等價
    1. 這兩個圖是等價的
        ```
        G1:  A → B → C
        G2:  A ← B → C
        ```  
        * Skeleton 都是一樣的：A—B—C ✅
        * 看 v-structure：
            * G1 沒有 v-structure（因為沒有節點同時有兩個父）
            * G2 也沒有 v-structure ✅
                → 因此 G1 與 G2 **等價**。

    2. 這兩個圖不等價
        ```
        G3:  A → C ← B
        G4:  A → C → B
        ```
        * Skeleton 一樣（A–C–B）✅
        * 但：
            * G3 有一個 v-structure：A→C←B ✅
            * G4 沒有 v-structure ❌
                → 所以它們 **不等價**。

### (OK) (42B P.44) Essential Graph
For a given DAG, an edge becomes bi-directional in the essential graph if there is an equivalent DAG in which the direction of the edge is reversed.

- 範例習題
exercise 4 - question 5 給一個 DAG，要你找出他的 essential graph。

### (OK) (42B P.36,42)Hill-Climbing Algorithm 有哪些規則?  
範例習題：Exc 5 - Qusestion 3 - d
1. **必須保持 DAG（有向無環圖）**
   – 無論是新增、刪除或反轉邊，最後得到的圖都必須沒有有向環（cycle）  
   – 因此：  
    - 新增一條邊 $i\to j$ 前，要檢查是否會形成從 $j$ 回到 $i$ 的有向路徑（若會，禁止新增）。  
    - 反轉一條已存在的邊 $i\to j$（變成 $j\to i$前)，也要檢查反轉後是否會造成環（若會，禁止反轉）。

2. **不能建立重複邊或自環**
   – 不能加已存在的有向邊或加 $i\to i$（自環）。
   – 刪除只能對已存在的邊做（不能刪不存在的邊）。
   – 反轉只能對已存在的邊做（不能對不存在的邊反轉）。

3. **單次鄰居操作通常只改動一條邊**
   – 在 hill-climbing / 局部搜尋裡，一個 neighbour 通常是對目前圖做「新增一條邊」或「刪除一條邊」或「反轉一條邊」其中一種、只改一條邊。

舉例：exercise 5 - question 5 會有習題問說：如果這一步驟做了某個動作（新增、刪除、反轉），下一步走的score有哪些需要重算？  

因為score只取決於node的parents集合，所以只需要重新計算對那個node的所有動作就好。ex: add(?? -> node), remove(?? -> node), reverse(?? -> node)  

只要那個操作改變了某個 node 的 parents，就要重算所有「連到那個 node」的 add / remove / reverse 的分數。

## Doubt
### Exc 5
#### Exc 5 - Question 2 - e
- 為何 Δdim=1

### HW3
#### HW3 - Question 3 - C

### HW4
#### HW4 - Question 4