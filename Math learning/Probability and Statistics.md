# 概率论和数理统计

本部分笔记部分参考 Dimitri P. Bertsekas & John N. Tsitsiklis 的著作*Introduction to Probability*，并同时参考其他资料整理

本部分的概念不能深究，因为大都不严谨。

## 样本空间与概率

### 集合

集合是一些对象的集合，这些对象称为集合的元素。

#### 集合的表示方法

- 列举法：$A = \{a_1, a_2, \dots, a_n\}$
- 描述法：$A = \{x | x \text{满足某种性质}\}$

**空间**：所有可能出现的元素的集合，记为$\Omega$。

#### 集合与集合的关系和运算

- $A \subset B$：$A$是$B$的子集
- $A \supset B$：$A$是$B$的超集
- $A = B$：$A$与$B$相等
- $A \cap B$：$A$与$B$的交集
- $A \cup B$：$A$与$B$的并集
- 对于多个集合的交集和并集，可以用$\bigcap$和$\bigcup$表示，如:
  $$\bigcap\limits_{i=1}^n A_i = A_1 \cap A_2 \cap \dots \cap A_n$$
  $$\bigcup\limits_{i=1}^n A_i = A_1 \cup A_2 \cup \dots \cup A_n$$

#### 集合的代数性质（都相对好理解）

- 交换律：$A \cap B = B \cap A$，$A \cup B = B \cup A$
- 结合律：$(A \cap B) \cap C = A \cap (B \cap C)$，$(A \cup B) \cup C = A \cup (B \cup C)$
- 分配律：$A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$，$A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$
- 互斥律：$A \cap B = \emptyset$，$A \cup B = \Omega$
- 补集律：$A \cup A^c = \Omega$，$A \cap A^c = \emptyset$
- 对偶律：$(A \cap B)^c = A^c \cup B^c$，$(A \cup B)^c = A^c \cap B^c$
- 吸收律：$A \cap (A \cup B) = A$，$A \cup (A \cap B) = A$
- **德摩根定律**：$(A \cap B)^c = A^c \cup B^c$，$(A \cup B)^c = A^c \cap B^c$

### 概率模型

- **样本空间**：所有可能出现的元素的集合，记为$\Omega$。
- **事件**：样本空间的子集，记为$A$。
- **概率**：事件$A$发生的可能性，记为$P(A)$。

#### 概率的性质

- 非负性：$P(A) \geq 0$
- 规范性：$P(\Omega) = 1$
- 可列可加性：对于任意可列个互斥事件$A_1, A_2, \dots$，有：
  $$P(\bigcup\limits_{i=1}^\infty A_i) = \sum\limits_{i=1}^\infty P(A_i)$$

#### 古典概型

离散概率：$$P({s_1, S_2, \dots , s_n}) = P(s_1) + P(s_2) + \dots + P(s_n)$$

样本空间$\Omega$中的元素有限，且每个元素发生的可能性相同，即：
  $$P(A) = \frac{事件A试验结果数量}{样本空间的等可能试验结果数}$$

#### 几何概型

连续概率：$$P(A) = \frac{A的面积}{样本空间的面积}$$

样本空间$\Omega$中的元素是连续的，且每个元素发生的可能性相同，即：
  $$P(A) = \frac{A的面积}{样本空间的面积}$$

#### 概率的基本性质

- 若$A \subset B$，则$P(A) \leq P(B)$
- $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
- $P(A \cup B) \leq P(A) + P(B)$
- $P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(A \cap B) - P(A \cap C) - P(B \cap C) + P(A \cap B \cap C)$  （**容斥原理**）

### 条件概率

#### 条件概率的定义

事件$B$发生的条件下，事件$A$发生的概率，记为$P(A|B)$，定义为：
  $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$

#### 乘法公式

$$P(A_1 \cap A_2 \cap \dots \cap A_n) = P(A_1)P(A_2|A_1)P(A_3|A_1 \cap A_2) \dots P(A_n|A_1 \cap A_2 \cap \dots \cap A_{n-1})$$

#### 全概率公式和贝叶斯定理

- 全概率公式：$$P(A) = \sum\limits_{i=1}^n P(A|B_i)P(B_i)$$
- 贝叶斯定理：$$P(B_j|A) = \frac{P(A|B_j)P(B_j)}{\sum\limits_{i=1}^n P(A|B_i)P(B_i)}$$

对于贝叶斯定理，可以理解为：

一组互不相容的事件，看作为对于样本空间的一个分割，即$B_1 \cup B_2 \cup \dots \cup B_n = \Omega$，则：
  $$P(B_j|A) = \frac{P(A|B_j)P(B_j)}{P(A)}$$

再将分母用全概率公式展开，即可得到贝叶斯定理。

这个公式的一个作用是**因果推理**，有若干的原因可以达到某个结果，通过这个推断原因的时候，可以假定$A_1, A_2, \dots A_n$为若干个原因，而$B$为结果，通过贝叶斯定理，可以计算出每个原因的概率。这个时候，称$P(A_i|B)$为**后验概率**，$P(A_i)$为**先验概率**。

### 独立性

#### 独立性的定义

事件$A$和事件$B$相互独立，当且仅当：
  $$P(A \cap B) = P(A)P(B)$$

如果$P(B) \neq 0$，则上式等价于：
  $$P(A|B) = P(A)$$

条件独立性：事件$A$和事件$B$在事件$C$发生的条件下相互独立，当且仅当：
  $$P(A \cap B|C) = P(A|C)P(B|C)$$

多个事件的独立性：事件$A_1, A_2, \dots, A_n$相互独立，当且仅当对于任意的$1 \leq i_1 < i_2 < \dots < i_k \leq n$，有：
  $$P(A_{i_1} \cap A_{i_2} \cap \dots \cap A_{i_k}) = P(A_{i_1})P(A_{i_2}) \dots P(A_{i_k})$$

需要注意的是，两两独立和相互独立是不同的，两两独立是指任意两个事件都相互独立，但是整体不一定独立。任意个数的事件出现与不出现，并不能影响其中任何一个或一部分事件的概率，这个时候，这些事件是相互独立的。 <!--这个地方有点难表述-->

#### 独立试验、伯努利实验和二项概率

- 独立试验：试验的结果只有两种可能，即成功和失败，记为$S$和$F$，且$P(S) = p$，$P(F) = 1 - p$。
- 伯努利实验：独立试验重复进行，每次试验结果相互独立。
- 二项概率：$n$次伯努利实验中，成功的次数为$k$的概率，记为$P_n(k)$，即：
  $$P_n(k) = C_n^kp^k(1-p)^{n-k}$$

### 计数法

#### 排列

从$n$个元素中取出$k$个元素，且考虑元素的顺序，称为从$n$个元素中取出$k$个元素的排列，记为$A_n^k$，即：
  $$A_n^k = n(n-1)(n-2)\dots(n-k+1) = \frac{n!}{(n-k)!}$$

#### 组合

从$n$个元素中取出$k$个元素，且不考虑元素的顺序，称为从$n$个元素中取出$k$个元素的组合，记为$C_n^k$，即：
  $$C_n^k = \frac{A_n^k}{k!} = \frac{n!}{k!(n-k)!}$$

  也可以简单记作：
  $$\binom{n}{k} = \frac{n!}{k!(n-k)!}$$

#### 分割

将$n$个元素分成$k$组，每组至少有一个元素，称为将$n$个元素分成$k$组的分割，记为$S(n, k)$，即：
  $$S(n, k) = \frac{1}{k!}\sum\limits_{i=0}^k(-1)^iC_k^i(n-i)^k$$

  或者简单记作：

  $$\binom{n}{n_1 , n_2 , \dots n_k} = \frac{n!}{n_1!n_2!\dots n_k!}$$

#### 二项式定理

$$(a+b)^n = \sum\limits_{k=0}^nC_n^ka^kb^{n-k}$$
