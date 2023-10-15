# 数据结构自学笔记

> 本内容就不多说废话了，基本上都是刷题的时候，一边看一边学的，也不算成系统，总之参考一下。最基本的内容就全都略过，从比较复杂（我觉得需要记录一下的东西开始）

## 0.预先章节

从C(C89甚至ANSI C)语言转到C++(C++ 14)，需要增加的一些内容，仅用于算法学习和实现，不涉及工程问题的“刚好够用”的补充。

### 0.0 从C到C++（C++89对比C89，增加的“刚好够用”的补充）

#### 0.0.1 bool

bool 类型是 C++ 中的基本类型，只有两个值：true 和 false。C89 中没有 bool 类型，但是可以用 int 类型代替 bool 类型，0 代表 false，非 0 代表 true。

```C++
// Using C++ 14
// bool
bool a = true;
bool b = false;
```

#### 0.0.2 nullptr

nullptr 是 C++ 中的关键字，表示空指针，可以用于任何指针类型。C89 中没有 nullptr 关键字，但是可以用 NULL 宏代替 nullptr 关键字。

```C++
// Using C++ 14
// nullptr
int* a = nullptr;
int* b = NULL;  // Not recommended in C++ language
```

#### 0.0.3 const

const 是 C++ 中的关键字，表示常量，可以用于任何类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。C89 中没有 const 关键字，但是可以用 #define 宏代替 const 关键字。

### 0.0.4 constexpr

`constexpr` 是 C++ 中的关键字，表示常量表达式，可以用于任何类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。C89 中没有 `constexpr` 关键字，但是可以用 #define 宏代替 `constexpr` 关键字。

### 0.0.5 static_cast

`static_cast` 是 C++ 中的关键字，表示静态类型转换，可以用于任何类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。C89 中没有 `static_cast` 关键字，但是可以用 `(type)` 表达式代替 `static_cast` 关键字。

### 0.0.6 dynamic_cast

`dynamic_cast` 是 C++ 中的关键字，表示动态类型转换，可以用于任何类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。C89 中没有 `dynamic_cast` 关键字，但是可以用 `(type)` 表达式代替 `dynamic_cast` 关键字。

> dynamic_cast 用于类的继承关系中，static_cast 用于基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。

### 0.1 C++ 14、C++ 11、C++ 03带来的新特性

#### 0.1.1 auto

auto 关键字用于自动推导变量类型，可以用于任何类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。使用时必须注意，auto 推导的变量类型与初始化表达式的类型完全一致，auto 推导的变量类型不会进行任何的隐式转换。

```C++
// Using C++ 14
// auto
auto a = 1; // int
auto b = 1.0; // double
auto c = 1.0f; // float
auto d = 1.0L; // long double
auto e = 1LL; // long long
auto f = 1u; // unsigned int
auto g = 1ul; // unsigned long
auto h = 1ull; // unsigned long long
auto i = 1.0e-10; // double
auto j = 1.0e-10f; // float
auto k = 1.0e-10L; // long double

auto l = 'a'; // char
auto m = "abc"; // const char*
auto n = true; // bool
auto o = nullptr; // nullptr_t

auto p = std::string("abc"); // std::string
auto q = std::vector<int>{1, 2, 3}; // std::vector<int>
auto r = std::map<int, std::string>{{1, "a"}, {2, "b"}}; // std::map<int, std::string>
auto s = std::make_pair(1, "a"); // std::pair<int, const char*>
auto t = std::make_tuple(1, "a", 1.0); // std::tuple<int, const char*, double>
```

#### 0.1.2 decltype

decltype 关键字用于获取表达式的类型，可以用于任何类型的表达式，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。使用时必须注意，decltype 获取的表达式类型与表达式的类型完全一致，decltype 获取的表达式类型不会进行任何的隐式转换。

```C++
// Using C++ 14
// decltype
int a = 1;
decltype(a) b = 1; // int

int* c = &a;
decltype(c) d = &a; // int*

int& e = a;
decltype(e) f = a; // int&
decltype((a)) g = a; // int&
decltype(++a) h = a; // int&
decltype(a++) i = a; // int

int j[10];
decltype(j) k = j; // int[10]

int l(int a, int b) { return a + b; }
decltype(l) m = l; // int(int, int)

auto n = [](int a, int b) { return a + b; };
decltype(n) o = n; // lambda

struct P {
    int a;
    int b;
};
decltype(P::a) p = 1; // int
decltype(P::b) q = 1; // int

template <typename T>
struct Q {
    T a;
    T b;
};
decltype(Q<int>::a) r = 1; // int
decltype(Q<int>::b) s = 1; // int

std::vector<int> t{1, 2, 3};
decltype(t) u = t; // std::vector<int>
decltype(t.begin()) v = t.begin(); // std::vector<int>::iterator
decltype(t.end()) w = t.end(); // std::vector<int>::iterator
decltype(t.cbegin()) x = t.cbegin(); // std::vector<int>::const_iterator
decltype(t.cend()) y = t.cend(); // std::vector<int>::const_iterator
decltype(t.rbegin()) z = t.rbegin(); // std::vector<int>::reverse_iterator
decltype(t.rend()) aa = t.rend(); // std::vector<int>::reverse_iterator
decltype(t.crbegin()) bb = t.crbegin(); // std::vector<int>::const_reverse_iterator
decltype(t.crend()) cc = t.crend(); // std::vector<int>::const_reverse_iterator
```

#### 0.1.3 lambda

lambda 表达式用于定义匿名函数，可以用于任何类型的表达式，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。使用时必须注意，lambda 表达式的类型与表达式的类型完全一致，lambda 表达式的类型不会进行任何的隐式转换。

```C++
// Using C++ 14
// lambda
auto a = [](int a, int b) { return a + b; };
// int(int, int)
```

### 0.2 STL

#### 0.2.1 std::vector

std::vector 是 C++ 中的容器，用于存储任意类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。std::vector 是一个动态数组，可以动态的增加和删除元素，可以动态的改变容器的大小。

```C++
// Using C++ 14
// std::vector
std::vector<int> a{1, 2, 3};
```

vector 的成员函数：

```C++
std::vector<int> a{1, 2, 3};

// 获取容器的大小
a.size(); // 3

// 获取容器的最大容量
a.max_size(); // 4611686018427387903

// 判断容器是否为空
a.empty(); // false

// 获取容器的第一个元素
a.front(); // 1

// 获取容器的最后一个元素
a.back(); // 3

// 获取容器的第 i 个元素
a[i]; // 1

// 获取容器的第 i 个元素
a.at(i); // 1

// 获取容器的第一个元素的迭代器
a.begin(); // std::vector<int>::iterator

// 获取容器的最后一个元素的迭代器
a.end(); // std::vector<int>::iterator

// 获取容器的第一个元素的常量迭代器
a.cbegin(); // std::vector<int>::const_iterator

// 获取容器的最后一个元素的常量迭代器
a.cend(); // std::vector<int>::const_iterator

// 获取容器的第一个元素的反向迭代器
a.rbegin(); // std::vector<int>::reverse_iterator

// 获取容器的最后一个元素的反向迭代器
a.rend(); // std::vector<int>::reverse_iterator
```

#### 0.2.2 std::map

std::map 是 C++ 中的容器，用于存储任意类型的变量，包括基本类型、指针、引用、数组、函数、lambda、类、模板、STL容器、STL迭代器、STL算法等等。std::map 是一个关联容器，可以根据 key 快速的查找 value，key 必须是唯一的，不能重复。

## 1. 二叉树

### 1.1 二叉树的遍历

#### 1.1.1 递归遍历

```C++
// Using C++ 14
// Preorder
void preorder(TreeNode* root) {
    if (root == nullptr) return;
    cout << root->val << endl;
    preorder(root->left);
    preorder(root->right);
}

// Inorder
void inorder(TreeNode* root) {
    if (root == nullptr) return;
    inorder(root->left);
    cout << root->val << endl;
    inorder(root->right);
}

// Postorder
void postorder(TreeNode* root) {
    if (root == nullptr) return;
    postorder(root->left);
    postorder(root->right);
    cout << root->val << endl;
}
```

## 一些题目和模型

### Top K

一个数组，求出其中最小的前 N 个数。

思路：

1. 用一个最大堆，维护前 N 个数，每次遍历到一个数，如果比堆顶小，就把堆顶弹出，把这个数放进去，最后堆里面就是最小的前 N 个数。
2. 快速排序，但是由于只需要前 N 个数，在快速排序的时候，如果左边的数的个数大于 N，就只对左边的数进行快速排序。
3. 使用STL中的nth_element函数，这个函数的作用是把第 N 小的数放在第 N 个位置上，左边的数都比它小，右边的数都比它大。

