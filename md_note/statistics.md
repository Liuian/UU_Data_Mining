## What is **deviance**?

## (OK) How to calculate degree of freedom for saturated model and independence model?
假設表格有 `r * c` 個格子，而這些格子相加總量一定要是 `N`。
這個表格「觀察資料的自由資訊數量」為 `r * c - 1`，因為最後一個格子的數量以經被限制為 `N - {所有其他格子的總和}`。

### Saturated model
這個模型沒有任何限制，唯一的限制是所有直相加依然要是 `N`。
因此這個模型「模型要估計的參數數量」為 `r * c - 1`。
自由度 = 「觀察資料的自由資訊數量」 -「模型要估計的參數數量」= `(r * c - 1) - (r * c - 1)` = `0`。

### Independence model
這個模型假設 `pij = pi * pj`。
每一row都可以指定一個比例，但是所有row的值相加要是1，因此對於row有 `r - 1` 個自由參數。
每一column都可以指定一個比例，但是所有column的值相加要是1，因此對於column有 `c - 1` 個自由參數。
因此模型「總共要估計的參數數量」為 `(r - 1) + (c - 1)`。
自由度 = 「觀察資料的自由資訊數量」 -「模型要估計的參數數量」 = `(r * c - 1) - ((r - 1) + (c - 1))` = `(r - 1)(c - 1)`

### Example
Data mining exercise 3, question 6:

Given a three-way table (`Program` × `Gender` × `Admission`) and independence graph `G⊥A|P`, what is the degree of freedom for the saturated model and the independence model?


1. **「觀察資料的自由資訊數量」為：**

    | 變數        | 符號 |        層級數       |
    | :-------- | :- | :--------------: |
    | Program   | P  |     2 (A, B)     |
    | Gender    | G  | 2 (Male, Female) |
    | Admission | A  |    2 (Yes, No)   |

    三個變數，都是 2 個層級。三維表有 ( 2 × 2 × 2 = 8 ) 個格子。  
    所以是 8 - 1 = 7 個自由資訊

2. **Saturated model 「要估計的參數數量」為。**

    $
    \log n_{PGA} = \lambda + \lambda^P_P + \lambda^G_G + \lambda^A_A + \lambda^{PG}*{PG} + \lambda^{PA}*{PA} + \lambda^{GA}*{GA} + \lambda^{PGA}*{PGA}
    $

    這裡有：
    * 一階項（main effects）：P, G, A
    * 二階交互作用（pairwise interactions）：PG, PA, GA
    * 三階交互作用：PGA

    全部7個

3. **Independence model 「要估計的參數數量為」：**

    > 在給定 P 之後，G 與 A 獨立。

    對 log-linear 模型的意思是：  
    * 要保留 P 與其他變數的關聯（PG, PA）
    * 但不允許 G 與 A 有直接關聯（GA）
    * 也不允許三重交互作用 (PGA)（因為那會讓「條件獨立」被破壞）

    所以這個模型包含的項是：

    $
    \log n_{PGA} = \lambda + \lambda^P + \lambda^G + \lambda^A + \lambda^{PG} + \lambda^{PA}
    $

    （沒有 $\lambda^{GA}$，也沒有 $\lambda^{PGA}$）


    * 主效應：P(1自由度)、G(1)、A(1) → 3 個
    * 二階交互：PG(1×1=1)、PA(1×1=1) → 2 個

    總共要估計的數量為 3 + 2 = 5個

**Ans:**
- Saturated model degree of freedom = 7 - 7 = 0
- Independence model degree of freedom = 7 - 5 = 2
- Degree of freedom for chi-square test = 2 - 0 = 2

**另一個角度看：**
- Saturated model 的所有 log-linear 項：P, G, A, PG, PA, GA, PGA
- Independence model 的所有 log-linear 項：P, G, A, PG, PA（少了GA, PGA）
- Independence model 少了 2 個項，因此 df 的差值為 2。



## What is the theorem of Chi-square test?

## What is the meaning of likelihood?

## (OK) **Useful Properties of Expectation and Variance**


1. Expectation（期望）的性質**
    |  #  | 公式   | 說明   |
    | :-: | -------------------------------- | ------------ |
    | (1) | $E(c) = c$ | 常數的期望值就是常數本身。                       |
    | (2) | $E(cX) = c , E(X)$ | 期望對常數具有線性可縮性。                       |
    | (3) | $E(X \pm Y) = E(X) \pm E(Y)$ | 期望是**線性運算子（linear operator）**，可加可減。 |
    | (4) | 若 $Z = c_0 + \sum_{i=1}^{n} c_i X_i$，則 <br> $E(Z) = c_0 + \sum_{i=1}^{n} c_i E(X_i)$ | 多變量線性組合的期望等於係數加權的期望和。 |

2. Variance（變異數）的性質**
    |  #  | 公式  | 說明   |
    | :-: | ----------------- | --------- |
    | (5) | $V(c) = 0 $   | 常數的變異數為零，因為沒有波動性。             |
    | (6) | $V(cX) = c^2 V(X)$   | 隨機變數乘上常數 $c$，變異數會乘上 $c^2$。    |
    | (7) | $V(X \pm Y) = V(X) + V(Y)$，若 $X$ 與 $Y$ **獨立**   | 兩個獨立變數的變異數可直接相加，符號 ± 不影響。 |
    | (8) | 若 $Z = c_0 + \sum_{i=1}^{n} c_i X_i$，且 $X_i$ **互相獨立**，則 <br> $V(Z) = \sum_{i=1}^{n} c_i^2 V(X_i)$ | 線性組合的變異數是各變數變異數的加權和（權重為係數平方）。 |

3. 符號解釋（Symbols Explanation）**
    |        符號       | 名稱                        | 意思                                        |
    | :-------------: | :------------------------ | :---------------------------------------- |
    | $E(\cdot)$    | Expectation（期望值）          | 隨機變數的平均或期望。 |
    | $V(\cdot)$    | Variance（變異數）             | 隨機變數的離散程度或波動性。 |
    | $X, Y, X_i$   | Random Variables（隨機變數）    | 表示隨機實驗的結果。 |
    | $c, c_0, c_i$ | Constants（常數）             | 固定不變的數值。 |
    | $Z$           | Linear Combination（線性組合）  | 定義為 $Z = c_0 + \sum_{i=1}^{n} c_i X_i$ |
    | $n$           | Number of Variables（變數個數） | 總共有 (n) 個隨機變數。 |
    | $X_i$ 互相獨立 | Mutually Independent      | 任何兩個 $X_i, X_j$ 間無相關性 $Cov = 0$。 |

4. 直覺總結**
    * **期望（Expectation）** → 衡量「平均」，可線性拆開。
    * **變異數（Variance）** → 衡量「波動」，常數倍會平方放大。
    * **獨立性（Independence）** → 保證變異數能直接相加，不需考慮共變異數。