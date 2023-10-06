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
- 德摩根律：$(A \cap B)^c = A^c \cup B^c$，$(A \cup B)^c = A^c \cap B^c$

### 概率模型

- **样本空间**：所有可能出现的元素的集合，记为$\Omega$。
- **事件**：样本空间的子集，记为$A$。
- **概率**：事件$A$发生的可能性，记为$P(A)$。

#### 概率的性质

- 非负性：$P(A) \geq 0$
- 规范性：$P(\Omega) = 1$
- 可列可加性：对于任意可列个互斥事件$A_1, A_2, \dots$，有
  $$P(\bigcup\limits_{i=1}^\infty A_i) = \sum\limits_{i=1}^\infty P(A_i)$$

