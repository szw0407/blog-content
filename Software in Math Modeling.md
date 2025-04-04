# 数学建模对应的软件基础 - 工具的选用

## 前言

数学建模并不是为了竞赛而存在的，但是数学建模竞赛可能是绝大多数人了解这个领域的契机。许多人对于这个竞赛的感知仅限于，需要用如此这般的软件，进行编程计算结果然后写个论文作为作答。但是实际上，尤其是在 LLM 已经具有很强的能力的背景下，这个竞赛的重心越来越倾向于**思路**的阐述、**论文**的撰写，而和程序编写能力等关系**越来越小**。许多脏活累活在如今看来可以直接交由 AI 完成，而竞赛者的差距越来越体现于对问题的理解和分析能力。

本文经过了一个非常巨大的删改，原先作者希望给出各种具体的解决问题思路的代码作为例子，但是现在写代码已经越来越不成为问题了，任何有能力把问题阐述清楚的人都可以使用 LLM 等工具生成完全可以使用的、准确而有可读性的代码。编程者在竞赛中的压力，乃至于必要性得到了特别大的下降，最终甚至可以尝试让 LLM 作为编程者，而队员可以全部致力于查阅资料、分析思路、整理数据等工作。

当然了，这个文章的主要内容还是讲的软件的使用，目的是让大家不至于在“得到了代码”的情况下没办法运行，以及帮助大家稍微了解一下这些软件是做什么的，在口口相传的神话里面祛魅，不至于被某些号称自己可以“熟练使用xxx、有获得过奖项”而就在组队群里面自夸的人赚取了信息差。考虑到 Python 语言已经被特别广泛地教学，而许多人对于 MATLAB 感到陌生，以至于错误地觉得“能熟练使用MATLAB、能熟练写$\LaTeX{}$”就意味着“有竞赛的实力”，花了特别大量的精力在这个上面是非常不值得的。本文内有大量的内容大概介绍了那个软件是什么，大概可以拿来干什么，以及为什么需要使用这个软件，本质也不过是祛魅，以及帮助选取合适的软件。

简单来说，MATLAB 就是一个计算器，只是计算的东西不限于加减乘除，而有更复杂的计算能力，也可以自定义函数去做计算，尤其擅长做矩阵有关的大规模数据计算，以及符号计算解题。可以在随后的正文里面了解这个。作者的观点是，“只会实现代码”的人在数学建模竞赛里面作用已经可以说被 LLM 取代了，参赛者**必须具有解题能力**，而不是**会写代码**甚至**会用软件**。因为**会用软件**只要看完本文基本上就行了，而**解题能力**是需要长时间的学习、实践中的积累和提高的。

值得注意的是，不论是Python，R，MATLAB，FORTRAN，F#，Mathematica等，都只是工具。工具的作用是**提高效率**、**解决问题**，不必为了使用工具而使用工具，应当把注意力放在算法逻辑上，并选用你认为**最合适、最便捷、最熟悉**的工具解决对应的问题。有一个古话说不“以词害意”，抓住重点，把准备的实践放在真正有用的地方。

另外，论文写作很多人推荐使用$\LaTeX{}$ ，也有人推荐使用简单的工具如MS Word等。本人对此的建议还是和上一条一样，选用你认为最熟悉、最合适的工具。如果使用$\LaTeX$，可以使用[TeX Live](https://tug.org/texlive/)配合[Visual Studio Code](https://code.visualstudio.com)+[Tex Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)等插件提供自动补全功能，或者配合GitHub Copilot等AI工具更智能地完成补全，甚至可以在[GitHub](https://github.com)下载现成的模板，节约大量的时间。对于这部分内容在正文里面会展开。

需要注意的是，如果使用Word，输入公式的最简单的办法是（测试版本为Word 2021/ Microsoft 365）：

1. 在准备插入公式的地方按<kbd>Alt</kbd>+<kbd>=</kbd>
2. 查看上方“公式”栏，默认选中了$\LaTeX{}$ 模式
3. 直接输入$\LaTeX$ 代码，比如类似这样的：`\begin{matrix}\frac{2}{x}&\alpha\\21&\pi^{-1.5}\\\end{matrix}`
4. 输入完成后按<kbd>Enter</kbd>
5. 你的公式立刻会渲染出来

在很久以前，MS Word对于公式的支持没有这么好的时候，由于数学建模竞赛的论文中公式非常多，使用Word排版很不容易。如，上面例子例给的代码，Word 2021能正确渲染成：$\begin{matrix}\frac{2}{x}&\alpha\\21&\pi^{-1.5}\\\end{matrix}$，这在Word 2016甚至更早的版本中是无法正确渲染的。那时候大家普遍使用的是Mathtype这一个非常昂贵的软件，可以用$\TeX$或者GUI输入公式并作为对象插入到Word中。现在，Word原生就能很方便处理公式了。此外，WPS里面内置了一个精简的类似Mathtype的公式编辑器，也可以通过MS Word调用。但是不推荐使用，因为其本质是插入了图片（对象），会影响排版的质量。

## 准备

本人使用的环境：

- MATLAB R2023b
- Python 3.10.12
- Anaconda
- Pycharm Community
- Visual Studio Code

此外，需要注意的是，很多内容都对使用终端、命令行有要求，可以事先入门一下 Powershell 或者 `cmd.exe`，至少知道那些是什么东西，大概是干什么的。

## MATLAB 使用

MATLAB是公认的最合适的数学建模工具之一，由于其极强的符号计算和很丰富的数据分析、仿真工具集，已经是工科乃至工业界不可取代的软件之一。

数学建模，尤其是在早期，其他工具很难做到如此一站式、易于使用、高性能，因此有特别大量的资料和教程都是以MATLAB为主的。

### MATLAB 基本操作的入门

MATLAB 是一种用于 算法开发 、 数据分析 和 可视化 、数值计算 的 科学计算语言 和 编程环境。

#### GUI操作

MATLAB的GUI是非常方便的，有的时候不需要代码即可完成很多的数据分析。

##### 数据绘图

- 绘制一个和多个曲线
- 绘制子图
- 导出图像

| 文件格式     | 使用场景         |
| -------- | ------------ |
| fig      | MATLAB       |
| bmp、tif  | 体积较大的无损格式    |
| JPEG、PNG | 电子照片压缩       |
| eps      | $\LaTeX{}$适合 |

##### 数据统计

- 基本拟合
- 曲线拟合
- 残差图和中心化缩放
- 拟合结果评价：**方差接近0为好；决定系数接近1为好**
- 多次拟合
- 调节范围确定最佳拟合，比如超过范围后单调性错误
- p1系数不能穿过0否则无意义，应选择低次数的，如$0.001\pm0.002$属于过拟合，直接最高次项系数取0
- 拟合结果保存

#### 命令行使用

下面是基本的样例

```MATLAB
>> %后的内容为注释
>> 1+3

ans = 

  4

>> x=1+3

x = 

  4

>> y=1+3; %结尾是分号则执行但是不输出结果
>> y %输出y的值

y = 

  4

>> % 变量区分大小写，以字母开头且只能有字母、数字和下划线
>> a=-.1; b=.1; % 小数的整数部分为0可以省略
>> d=1.6e-20; e=3.6E25; %科学计数法
>> h=1i; m=2e3j; %虚数的表示
>> eps(1) %double之间的微小的间隔限制了精度，用这个函数打印出1到下一个double的间距;可以验证，数字越大，间隔越大

ans = 

  2.2204e-16

>> format long;
>> a=pi %15位精度的π

a =

    3.141592653589793

>> 1/0

ans = 

  Inf

>> 0/0

ans = 

  NaN

>> a=sin(pi) %调用函数

a = 

    1.224646799147353e-16

>> max(1,2)

ans =

     2

>> max([3 4 5])

ans =

     5

>> [maxv,index]=max([3 4 5])

maxv =

     5


index =

     3

>> disp("Hi MATLAB")
Hi MATLAB
>> clear % 清除之前的所有变量
>> A=magic(8); B=rand(4,8);whos %显示工作区全部内容：A为一个幻方，B是一个4*8的随机数矩阵
  Name      Size            Bytes  Class     Attributes

  A         8x8               512  double              
  B         4x8               256  double              

>> save myvar.mat
>> clear
>> load myvar.mat
>> clc %清除命令行
>> exit %退出
```

还有一些简单的系统级指令，比如`ls` `pwd` `cd`之类，熟悉POSIX或者NT的命令行就能很容易掌握

获取帮助：输入命令后停顿等待提示、`help`显示简单文档和`doc`打开函数文档

***

#### MATLAB数据类型

到此为止，软件层面的简单操作已经基本入门了，下面的内容就面向于如何写MATLAB计算程式。

MATLAB的特色是数据都在使用matrix的方式处理。数据类型丰富，数据类型的设置和现代的编程语言很类似，命名稍有区别。

- logical
- char
- numberic
  - int8 int16 int32 int64
  - uint8 uint16 uint32 uint64
  - single
  - double
- table
- cell
- struct
- Function Handle(@)

#### 数组（向量和矩阵）及基本操作

##### 基础

```MATLAB
>> h=[1 2] % row vector

h =

     1     2

>> hh=[3,4] % also row vector

hh =

     3     4

>> v=[5;6] % column vector

v =

     5
     6

>> M = [1,2;3,4]

M =

     1     2
     3     4
>> c1=1:5

c1 =

     1     2     3     4     5

c2 =

     1     3     5     7     9

>> c3=0:pi/2:pi

c3 =

                   0   1.570796326794897   3.141592653589793

>> whos
  Name      Size            Bytes  Class     Attributes

  M         2x2                32  double              
  c1        1x5                40  double              
  c2        1x5                40  double              
  c3        1x3                24  double              
  h         1x2                16  double              
  hh        1x2                16  double              
  v         2x1                16  double              

>> 
```

其他常见的命令：

| MATLAB命令               | 作用                          |
| ---------------------- | --------------------------- |
| linspace(from,to,nums) | 从from到to等距离取nums个数值         |
| logspace(0,2,3)        | $10^0$到$10^2$找3个点，即1 10 100 |
| zeros                  | 零矩阵                         |
| ones                   | 1矩阵                         |
| eye                    | 单位矩阵                        |
| norm                   | 求向量的模                       |
| rand                   | 随机数矩阵                       |
| randn                  | 正态分布的随机数矩阵                  |
| randi                  | 有范围的随机数矩阵                   |
| randperm(x)            | 1~x之间的整数随机排序                |
| randperm(x,y)          | 1~x之间随机选择y个整数               |
| diag                   | 将对角线上面的数值返回为一个列向量           |

##### 索引

双下标索引：x行y列

单下标索引：从上到下、从左到右数

`end`代表当前行（或者全部）的末尾，`end-1`为倒数第二

索引取子矩阵：

```MATLAB
>> A=magic(3)

A =

     8     1     6
     3     5     7
     4     9     2

>> A(2,[1,3]) % [A(2,1),A(2,3)]

ans =

     3     7

>> A([2,3],[1,3]) % [A(2,1),A(2,3);A(3,1),A(3.3)]

ans =

     3     7
     4     2

```

##### 冒号表达式

`c=1:-0.5:0`表示从1到0步长为-0.5

```MATLAB
>> A=magic(3)

A =

     8     1     6
     3     5     7
     4     9     2

>> A(1:3:end)

ans =

     8     1     6

>> A(2,:)

ans =

     3     5     7

>> A(:,1)

ans =

     8
     3
     4

>> A(:,:)

ans =

     8     1     6
     3     5     7
     4     9     2

>> A(:)

ans =

     8
     3
     4
     1
     5
     9
     6
     7
     2

>> 
```

##### 数组操作

数组边界赋值可自动扩充数组，并在空位置补充0

```MATLAB
A=[];
A=[A 1];  % A=[1]水平串联
A=[A;[2;3]];   % A=[1;2;3]竖直串联
```

使用这个特性，数组可以构建表（比如列向量在行方向串联）

空矩阵可用于删除行或者列（用冒号索引选中对应的行、列赋值为空矩阵）

若要交换行列可以：`A(:,[1,3,2])`则输出的矩阵为A2 3列交换的矩阵

length函数输出array的长度

numel函数输出数组的元素数量

size函数输出的数组的x 和y 组成的行向量

sort将每一列按照从小到大的顺序排序；行向量则直接排序

flip翻转向量或者按列翻转矩阵

reshape为重构，将矩阵写入新的横纵比

sum函数求矩阵的和，矩阵元素球每一列的和并返回行向量；cumsum为累计求和返回行向量

prod求乘积，cumprod为累计求积

diff为差分

##### 特殊矩阵和相关方法

sprace 稀疏矩阵，仅存储非0的元素及其索引，用于非0元素极少而分布不规律的矩阵；
相关的方法：

- nnz：Non-zeors的数量
- nnz/numel（计算密度）
- nonzeros返回非0数字构成的列向量

多维数组：可由zeros、ones和rand等方法生成，给边界外赋值自动扩展

字符数组：以单引号括起的字符

元胞数组（cell）：可以包含任何数据类型，用大括号括起来。访问可用{x,y}访问坐标或者(:,1)取子集。
相关的方法：cell2mat将其转换为普通数组

结构体：每个字段都可以是任何数据类型，用`.`可访问字段；结构体本身可以构成数组

表（table）是一种内置的结构体
相关的方法：

- `T=table(x,y,z,etc)`创建一个表格
- `T.x`访问表中的内容
- 使用索引查询表的内容
- 可使用行名访问数据

#### 运算符号

##### 逐元素运算（element by element）

| 运算符号 | 操作                         |
| -------- | ---------------------------- |
| + -      | 加减                         |
| .*  ./   | 乘除                         |
| .^       | 乘方                         |
| .‘       | 非共轭转置（不改变虚部符号） |

##### 线性代数运算（linear algebra）

| 运算符号 | 操作                                      |
| -------- | ----------------------------------------- |
| *        | 乘法                                      |
| / \      | 除法，其中表示$Ax=B$中的$x=A\backslash B$ |
| '        | 复共轭转置                                |
| ^        | 矩阵幂运算                                |

##### 数值运算的Functions

round ceil floor：四舍五入、向上和向下取整

mod：求余数

##### 逻辑运算

```MATLAB
>> [1 2;3 4]>2

ans =

  2×2 logical 数组

   0   0
   1   1

>> A=[1 2 ;3 4]

A =

     1     2
     3     4

>> B=A>2

B =

  2×2 logical 数组

   0   0
   1   1

>> A(B)  % 使用logical索引，比如此处导出了A中所有大于2的数的列向量

ans =

     3
     4

>> find (A>2)  % 返回索引

ans =

     2
     4

>> A(ans)

ans =

     3
     4 

>>
```

##### 关系运算

运算符中~=代表不等于

数组的比较返回数组：

```MATLAB
>> [1 2 3]<[3 2 1]

ans =

  1×3 logical 数组

   1   0   0

>> 
```

isempty：确认是否为空

all：确认所有元素是否为0

any：确认是否有元素为0

与& 非~ 或| 异或xor

#### 函数操作

##### 函数的定义

```MATLAB
function output=func1(x)
output = x
end
%% OR declear:
% function [x,y,z]=func2(t)
% function func3()
% function []=func4(m,n)
```

返回值可以是0个，一个或多个；
传入参数也可以为0个，1个或多个；
函数名称只能以字母开头且包含字母、数字和下划线。

MATLAB的函数开始于`function`终止于`end`，循环`for`、判断`if`同理。

##### 函数的调用

MATLAB会在变量→当前文件夹下的函数→搜索路径中的函数，其中搜索路径可在设置中修改。

##### 匿名函数

只含有一个可运行语句的函数

```MATLAB
sqr = @(x) x.^2
```

这个语句是一个例子

##### 函数句柄

匿名函数本身就是一个句柄，而已有的函数创建句柄：

```MATLAB
integral(@sin, 0, 2*pi)  %计算sin在0到2π的积分
```

##### 逻辑语句和循环语句

```MATLAB
if STATEMENT
 func1()
elseif STATEMENT2
 func2()
else
 func3()
end

switch VAR
 case VALUE1
  func1()
 case {VALUE2,VALUE3}
  func2()
 otherwise
  func3()
end

for i=1:1:20
 func(i)
end

while STATEMENT
 func()
 if STATEMENT2
  break
 elseif STATEMENT3
  continue
 end
 func()
end

```

#### 脚本和实时脚本

为了保存一系列的命令，MATLAB提供脚本以存储程式，默认以`.m`文件格式。

文件内，`%`开头的为注释。

`%%`为区分代码节的符号，此行后的注释可以认为是代码节标题。

MATLAB代码可以很轻松发布，为HTML、PDF、office和$LaTeX{}$文件，并支持多种排版格式

实时脚本类似于jupyter notebook，可实时运行代码段并交互

在脚本中经常需要定义函数，并也可以选择某一个函数单独运行。

#### 调试和错误排查

MATLAB的IDE已经提供所有常见的功能了，本部分略过

#### 绘图

##### plot绘图

```MATLAB
plot(x,y,x,y,...%x-y的一一对应
 ,Name,Value,...%属性的键值对
 )
```

```MATLAB
x=0:pi/100:pi
y=sin(x)
plot(x,y)
```

这个程式绘制了y-x的图像

在刚才的程式里，如果是`plot(y)`，则绘制了索引为横坐标的图像（看到上面x每隔$\frac{\pi}{100}$取一次值了吗）

```MATLAB
x=0:pi/100:2*pi
plot(x,sin(x),...% 换行
 x,cos(x))
```

这个程式绘制了2个函数图像。

由于x实际上是一个行向量，而sin(x)也是一个行向量；plot实质上传入的是偶数个行向量，其中一个横坐标对应一个纵坐标。

```MATLAB
y=[sin(x)' cos(x)']; % 构建了一个matrix
plot(x,y)
```

也能完成一样的绘图效果。

每次plot都会清空之前的图像。如果需要保存图像：

```MATLAB
figure
hold on
plot(x,f(x))
plot(x,x)% 两个函数图像：y=x和y=f(x)
hold off
plot(x,x.*2)%清掉了之前绘制的图像
```

`plot`函数还可以传入线性、颜色和标记等，具体可以查阅文档。

```MATLAB
xlable('x')
ylable('e^x')% 这里的字符串是可以支持tex语法的，
title('Plot of e^x')
legend('sin')%这个是图例
```

还有更多的标注：

```MATLAB
[ymin,index_min] = min(y)
xmin=x(index_min)  %第indexmin个采样点对应的x
index_max = find(y==max(y))
ymax = y(index_max)
xmax = x(index_max)
t=['Min=',num2str(ymin)]
t2=['Max=',num2str(ymax)]
text(xmin,ymin,...%坐标位置填入文本
 ,t)
text(xmax,ymax,t2,'HorizontalAlignment','right')  %标在右侧而不是默认的左侧

text(2,2,'$$\int_{0}^{2} x^2\sin(x) dx $$',"Intepreter",'latex')
aixs equal   %横纵坐标取等距离为单位长度

axis([0 pi/2 0 5])  %设置x范围[0,pi/2]，y范围[0,5]

```

此外，还有绘制子图的`subplot`函数，可将窗口分为若干块并绘制多个图

##### 函数绘图

`fplot(fx, fy)`，其中fx  fy都是函数句柄，比如：`fx=@(x) cos(x);`

后面可以添加参数，如范围（默认为$[-5,5]$）、线型等和`plot`类似

`fimplicit(f)`绘制的是隐函数f(x,y)=0的图像，f参数为参数(x,y)的函数句柄

##### 常用的绘图函数

- semilogy：半对数图
- bar：柱状图
- histogram：直方图
- scatter：散点图
- polarplot：极坐标线图
- errorbar：误差条形图
- heatmap：热图
- wordcloud：词云
- geobubble：地理气泡图
- plot3：三维函数线图
- meshgrid：网格坐标矩阵
- surf：三维曲面图
- mesh：三维网格图
  etc.

#### 符号计算

符号计算相较于数值计算，优点在于：

- 高精度
- 支持多种复杂计算的求解
- 输出更易读

`sym`创建或将值转换为符号数。

可变精度计算（`vpa`）：默认32位的可指定精度计算，并能恢复p/q pπ/q 等的精度。

`syms`可创建符号变量，可通过符号变量创建符号矩阵和符号函数

常见的计算functions：

- 极限limit
- 求导diff
- 积分int
- 级数求和symsum
- 化简simplify
- 替换subs
- 绘图fplot
  etc.

代数方程求解solve可求解符号方程如`solve(ax^2==b)`

或者`solve(ax^3+x)`求解多项式（默认等于0）

或者`solve(x^2+y^3-1,y)`求解二元隐函数

solve还可以求解线性方程组：

```MATLAB
solve(EQUATION1, EQUATION2, EQUATION3)
%% OR
eq=[EQUATION1, EQUATION2, EQUATION3];
S=solve(eq)
sol=[S.x; S.y]
%% OR
[A,b] = equationsToMatrix(eq,x,y)
z=(A\b)
```

`solve`可返回所有解和条件解

`vpasolve`返回数值求解（默认32位精度）

## Python 计算相关

Python 最初是一种脚本语言，由 Guido van Rossum 于 1989 年圣诞节期间编写，后续愈发强大，成为一种通用编程语言。木纹的主要版本是 Python 3，可以在 [Python 官网](https://www.python.org) 查看具体的版本信息，推荐使用的版本是稳定版本，最好是在 security fix 阶段的版本，不推荐使用 pre-release 阶段的版本。

此外目前有一个很大的问题，在 Python 3.13 中试验性地开放了 GIL，可能导致安装和使用一部分的包的时候出现一些奇怪的问题，比如需要重新编译，或者对于线程的使用有一些限制。因此，推荐使用 Python 3.12 或者更早的版本，直到这些包都能经过充分的测试可以稳定运行在 Python 3.13 上。

Python 的强大不完全在于其简洁易学的语法特征。受到很多限制，其性能相较于 FORTRAN、C、C++ 等语言有所不足。但是 Python 有很多优秀的库，比如 NumPy、SciPy、Pandas、Matplotlib 等。这套生态成为了 Python 的重大优势，目前几乎可以完成所有的科学计算任务，而且那些库完全开源，可以**自由**、**免费**地使用。这些库基本上是社区维护的，或许在可靠性、准确性上比商用软件有一些欠缺，并且使用这些库导致的错误不会有商用软件那样完备优质的技术支持和服务等，也不会有任何维护者对项目的使用出现问题而产生的任何代价负责；但是出现各种问题的概率其实很低，而且社区的维护者乃至社区的各种网友会热情而尽力地解决问题。

Python 默认安装的库非常有限，而绝大多数的库都在 PyPI 上。PyPI 是 Python Package Index 的缩写，是 Python 的第三方库的官方仓库。这些库开源通过 pip 安装。但是，由于可能存在各种库之间复杂的依赖关系，推荐使用的还是各种虚拟环境。

#### Python 虚拟环境

Python 的虚拟环境有很多种，比如 venv、virtualenv、Conda 等。对于熟悉的用户来说，任何一个工具，甚至包括不隔离成虚拟环境的特殊环境管理方案，比如 pdm、uv 等，都可以使用。但是对于初学者，以及不专注于开发或者运维的普通科学计算使用者来说，推荐使用 Conda。尤其是 Anaconda，这是一个集成了 Conda 的 Python 发行版，包含了很多科学计算相关的库，而且可以很方便地安装和管理库，对于那些希望开箱即用的人来说，甚至不需要手动安装大多数的库。

但是还是需要介绍一下Conda的使用。Conda 可以很好地管理和复用虚拟环境。创建虚拟环境的目的是为了隔离不同项目的依赖，避免版本冲突。

首先安装Miniconda，安装完成后在终端中输入这行命令（示例）：

```powershell
conda create -n $ENV_NAME python=3.10
```

即可创建一个Python 3.10的环境，名为`$ENV_NAME`。

对于虚拟环境，我们暂时还需要了解一些基本的操作。需要注意的是，许多包并没有在默认的 Anaconda 渠道中，需要在安装时指定渠道，比如 `-c conda-forge`。并且，由于部分版权原因和其他的原因，Anaconda 仓库的包并不能被用在许多场景下，可能有侵权风险。

```powershell
conda activate $ENV_NAME  # 激活环境
conda install $PACKAGE_NAME  # 安装包
conda install $PACKAGE_NAME=$VERSION  # 安装指定版本的包
conda install $PACKAGE_NAME1 $PACKAGE_NAME2  # 安装多个包
conda install -c $CHANNEL_NAME $PACKAGE_NAME  # 安装指定渠道的包
conda install -c $CHANNEL_NAME $PACKAGE_NAME=$VERSION  # 安装指定渠道的指定版本的包
conda remove $PACKAGE_NAME  # 删除包
conda list  # 列出当前环境下的所有包
conda deactivate  # 退出环境
conda env list  # 列出所有环境
conda env remove -n $ENV_NAME  # 删除环境
conda env export > environment.yml  # 导出环境
conda env create -f environment.yml  # 导入环境
conda create -n $NEW_ENV_NAME --clone $ENV_NAME  # 克隆环境
conda env update -f environment.yml  # 更新环境
conda env create -f environment.yml -n $ENV_NAME  # 导入环境并指定环境名
```

对于库具体的使用，需要查阅各自的文档。常用的库包括：

- NumPy：数值计算库
- SciPy：科学计算库
- Matplotlib：绘图库
- Pandas：数据处理，主要是表格数据
- SymPy：符号计算库，与某些其他的库兼容性不好，效果远不如MATLAB
- Scikit-learn：机器学习库

一些具体任务，比如线性规划、最优化、微分方程求解，可以使用 SciPy 的子库，比如 `scipy.optimize`、`scipy.integrate` 等。这部分内容查阅文档或者直接交给LLM解决就行，关键是给出具体的思路。

举一个例子，比如说对于一个函数求解零点问题，可以使用 `scipy.optimize.root` 函数。下面的程序可以求解非线性方程$\sin(x^2) \exp(-x/3) + \cos(\pi x) \log(|x| + 1) - 2.5 = 0$的零点。

首先安装 SciPy：

```powershell
conda create -n SampleEnv python=3.10 -c conda-forge
conda activate SampleEnv
conda install scipy
# 接下来根据提示输入 y 即可
```

然后运行下面的程序（可以直接在命令行打开 Python 解释器，然后逐行输入或者直接一把粘贴进去）：

```python
import numpy as np
from scipy.optimize import root

def func(x):
     return np.sin(x**2) * np.exp(-x/3) + np.cos(np.pi*x) * np.log(abs(x) + 1) - 2.5

sol = root(func, 0.5)
print(sol.x)
```

> 这部分内容有空再写吧，或许也不需要写了，有关的内容和资料太多了。

## 稍微写点关于 TeX 的东西

### 丑话说前面

先要明确一个事情，这个东西不能说完全没有必要，但是如果为了打竞赛“刻意”上$\LaTeX{}$，那是特别不合适的。很显然绝大多数人，甚至包括作者本人，虽然作者本人有能力直接阅读和编写$\LaTeX{}$代码，但是还是认为对于偶尔用一次两次的人而言**特别特别没有意义**。他仅限于那些拿着模板“完全不想花时间排版”，并且有能力就像说话和正常打字一样顺滑地编写$\LaTeX{}$代码，甚至觉得不使用这个东西反而会阻塞思路、影响效率的人。

对于**合格**的使用$\LaTeX{}$的人，他们一般具有这些特点：

- 可以用眼睛和脑子直接认识公式，把公式改写成$\LaTeX{}$代码或者反过来就和说话一样顺畅而不需要额外的思考
- 有精心配置的 IDE 或者文本编辑器，可以非常顺滑地编写冗长的$\LaTeX{}$关键词，比如`\begin{equation}`、`\end{equation}`、`\frac{}{}`、`\int_{}^{} f(x)\mathrm{d}x`等等。
- 有模板可以套用，可以很顺利而且美观地通过`section`、`subsection`等命令组织文档的层次结构
- 所有的内容，都使用$\LaTeX{}$编写，比如`matplotlib`产生的图像可以直接保存成兼容$\LaTeX{}$的格式，参考文献也是用的bibtex格式，生态完全不需要离开$\LaTeX{}$环境
- 能够很顺利地进行文献的排版，而杜绝了 Word 里面各种难以解决的格式、排版问题
- 使用比如说 git 管理全部的文件，可以基于文本内容进行版本控制和溯源等。

以上内容真的只是**合格**的标准，虽然对于一些外行来说匪夷所思，但是我相信对于$\LaTeX{}$党而言，是公认的要求，这也是为什么他们都会强烈要求其他人一起使用$\LaTeX{}$。对他们来说生产力工具的差异会严重干扰工作的正常进行，让人不适、效率堪忧。

并且恕我直言，绝大多数人使用 Word 也水平很低，对于细节的排版只能说马马虎虎能看，而且完全不会花时间对母版等进行预先的修改以便满足具体需求，甚至很多的工作都丑陋笨拙地挨个调节。但是熟悉的工具，哪怕格式确实有小瑕疵，最终不会严重影响内容的传达，而内容的优质才是竞赛的关键。$\LaTeX{}$的使用，对于很多人来说，只是增加了一个学习成本，反而降低了效率，得不偿失。最后$\LaTeX$成品的格式确实整齐，必要性有，但是投入产出比如何，自行斟酌。

除此以外，目前还有一个新近开发并且生态日渐成熟、语法更加先进的新的排版语言 typst，很多非常熟悉$\LaTeX$的人在尝试后都愿意切换到它上面，或许对于一部分人而言可以试试。

### 安装 TeX 编译环境

对，这个东西是需要相对复杂的安装的，而且很多人在安装中就遇到了许多严重的问题。

绝大多数人使用的是Windows系统。但是$\LaTeX$最初是在Unix系统上开发的，这二者之间存在特别大的差别，所以在Windows上安装$\LaTeX$环境是特别麻烦的。

在踩过非常多的坑之后，我还是建议任何希望使用 $\LaTeX$ 的人直接去安装一个 Linux 系统，然后通过 Linux 的包管理器安装 Tex Live。这样可以避免非常多的问题，而且 Linux 系统的使用对于科学计算和编程来说是非常有帮助的，可以顺便帮助解决许多环境配置的问题，唯一的可能不便的就是 MATLAB 的 Linux 版本并不是特别容易安装和使用，但是也只是稍许的有一些“权限上”的问题和图标上的瑕疵。

Windows环境下面安装 Tex Live的教程非常多，我就强调几个要点就行：

- 下载安装光盘直接从镜像下载，安装的过程中也需要切换镜像站，不然速度太慢
- 安装的路径和所有的环境变量都不可以有任何非ANSII的字符，可以在Windows环境变量里面查看，尤其是`%HOME%`、`%TEMP%`等，某些用户名里面有特殊字符的需要特别注意。全部改写，一劳永逸。
- 安装完成之后配置好PATH，然后推荐使用 VS code 安装 Latex workshop 插件使用，配合 GitHub Copilot 等 AI 工具使用更舒适。

此外，还可以尝试 MiKTeX 这个软件，或许会容易一些，最关键的是由于其根据需要安装包，就可以避免完整安装环境而占用巨大资源，安装了不必要的大量内容，甚至包括比如说排版五线谱、国际象棋的包，以及包括全世界各大大学的模板文件。此外 MiKTeX 也有一个很好的特性，就是可以自动下载缺失的包，而不需要手动去下载，还可以更方便地更新（而 Tex Live 的更新是非常麻烦的，基本上只能重装了），控制台的 GUI 也很容易使用。自定义安装 Tex Live 也可以避免这个，但是很麻烦。总之就是自己折腾呗。唯独有一个就是使用 xeLaTeX 编译的时候，可能需要手动再安装一个 perl 解释器，不然会报错。

### 简单的上手建议

对于新学习的人而言，不需要也没有时间去完整阅读文档。正确的使用方案是，`git clone`下来一个模板，比如说对于数学建模 CUMCM 竞赛，只需要`git clone https://github.com/latexstudio/CUMCMThesis.git`，或者 Fork 下来作为模板仓库，并以此创建新的仓库。

然后打开`example.tex`文件，删除正文的样例内容，填字，然后编译。编译根据要求，如果我没有记错的话是选用 xelatex 而不是默认的 PDF latex，否则无法支持中文（准确来说是CJK）字符。

对于绝大多数的使用者而言，不需要掌握复杂的 $\LaTeX$技巧也足以完成一个给定模板填写论文正文的任务。还是那句话，把时间花在更重要的事情上面，包括分析问题和解决问题的能力，以及阐述清楚思路的前因后果的能力，以便在竞赛里面脱颖而出。

## 总结和后记

数学建模竞赛的核心在于对问题的理解和分析能力，而不是对工具的熟练使用。本文介绍了 MATLAB 和 Python 软件本身的基本使用方法（甚至没有特别介绍语法等高深的内容），旨在帮助参赛者快速上手这些工具，提高效率。对于论文写作工具的选择，建议根据个人熟悉程度和实际需求进行选择，不必盲目追求某种工具。希望本文能为参赛者提供一些有用的参考，帮助大家在竞赛中取得好成绩。

本文的部分内容可能会在博客的其他地方查阅到更加详细的信息，或者被本人转载在其他平台。
