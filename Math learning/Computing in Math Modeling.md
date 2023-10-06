# 计算机与数学建模

## 前言（关于工具的选用）

目前，本人比较熟悉的工具更多是Python。由于数学建模竞赛中，很多（甚至可以说几乎市面上的全部）参考资料都默认使用MATLAB作为语言。由于MATLAB的一些特性，包括但不限于昂贵、体积巨大、不便在一些场景下部署，本人计划使用两种语言都给出代码案例，以供参考、对照。

数学建模并不是为了竞赛而存在的，因此这些代码和算法、模型可能可以在现实中得到应用；同时本人尽力提升代码的质量，让其本身可以成为MATLAB、Python等的优质代码样例参考。

值得注意的是，不论是Python，R，MATLAB，Fortran，F#，Mathematica，lingo等，都只是工具。工具的作用是**提高效率**，不必为了使用工具而使用工具，应当把注意力放在算法逻辑上，并选用你认为**最合适、最便捷、最熟悉**的工具解决对应的问题。

另外，论文写作很多人推荐使用$\LaTeX{}$ ，也有人推荐使用简单的工具如MS Word等。本人对此的建议还是和上一条一样，选用你认为最熟悉、最合适的工具。如果使用$\LaTeX$，可以使用[TeX Live](https://tug.org/texlive/)配合[Visual Studio Code](https://code.visualstudio.com)+[Tex Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)等插件提供自动补全功能，或者配合GitHub Copilot等AI工具更智能地完成补全，甚至可以在[GitHub](https://github.com)下载现成的模板，节约大量的时间

需要注意的是，如果使用Word，输入公式的最简单的办法是（测试版本为Word 2021/ Microsoft 365）：

1. 在准备插入公式的地方按<kbd>Alt</kbd>+<kbd>=</kbd>
2. 查看上方“公式”栏，默认选中了$\LaTeX{}$ 模式
3. 直接输入$\LaTeX$ 代码，比如类似这样的：`\begin{matrix}\frac{2}{x}&\alpha\\21&\pi^{-1.5}\\\end{matrix}`
4. 输入完成后按<kbd>Enter</kbd>
5. 你的公式立刻会渲染出来

在很久以前，MS Word对于公式的支持没有这么好的时候，由于数学建模竞赛的论文中公式非常多，使用Word排版很不容易。如，上面例子例给的代码，Word 2021能正确渲染成：$\begin{matrix}\frac{2}{x}&\alpha\\21&\pi^{-1.5}\\\end{matrix}$，这在Word 2016甚至更早的版本中是无法正确渲染的。那时候大家普遍使用的是Mathtype这一个非常昂贵的软件，可以用$\TeX$或者GUI输入公式并作为对象插入到Word中。现在，Word原生就能很方便处理公式了。此外，WPS里面内置了一个精简的类似Mathtype的公式编辑器，也可以通过MS Word调用。

## 准备

本人使用的环境：

- MATLAB R2023A
- Python 3.10.12, virtual environment created by Conda 4.10.3
- Pycharm Community 2022.3.3
- Visual Studio Code 1.80.0

## 基础使用

### MATLAB 基本操作的入门

MATLAB 是一种用于 算法开发 、 数据分析 和 可视化 、数值计算 的 科学计算语言 和 编程环境。

#### GUI操作

MATLAB的GUI是非常方便的，有的时候不需要代码即可完成很多的数据分析。

##### 数据绘图

- 绘制一个和多个曲线
- 绘制子图
- 导出图像

| 文件格式  | 使用场景           |
| --------- | ------------------ |
| fig       | MATLAB             |
| bmp、tif  | 体积较大的无损格式 |
| JPEG、PNG | 电子照片压缩       |
| eps       | $\LaTeX{}$适合     |

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

| MATLAB命令             | 作用                               |
| ---------------------- | ---------------------------------- |
| linspace(from,to,nums) | 从from到to等距离取nums个数值       |
| logspace(0,2,3)        | $10^0$到$10^2$找3个点，即1 10 100  |
| zeros                  | 零矩阵                             |
| ones                   | 1矩阵                              |
| eye                    | 单位矩阵                           |
| norm                   | 求向量的模                         |
| rand                   | 随机数矩阵                         |
| randn                  | 正态分布的随机数矩阵               |
| randi                  | 有范围的随机数矩阵                 |
| randperm(x)            | 1~x之间的整数随机排序              |
| randperm(x,y)          | 1~x之间随机选择y个整数             |
| diag                   | 将对角线上面的数值返回为一个列向量 |

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

### Python 计算相关

Python很适合做计算。目前做科学计算方面，Anaconda整合了很多常用的包。受此启发，但是由于Anaconda过于专业、体积庞大（虽然比起MATLAB这根本不算什么），我们选择使用Miniconda自行搭建环境。

首先介绍一下Conda的使用。

首先安装Miniconda，安装完成后在终端中输入这行命令（示例）：

```powershell
conda create -n $ENV_NAME python=3.10
```

即可创建一个Python 3.10的环境，名为`$ENV_NAME`。

了解环境搭建之后，我们可以开始学习计算相关的库。

#### Numpy

Numpy是一个线性代数计算库，提供了很多开箱即用的线性代数计算方法。

#### Sympy

Sympy是一个符号计算库，但是似乎与其他的库兼容性很不好，慎用。使用时，需要再计算完后转换数据为常规的类型。

#### Scipy

做科学计算的一个库，封装了很多算法和公式
#### pandas

pandas是一个基于numpy的数据处理库。其最大的作用就在于数据的读取。默认支持读取CSV文件，同时也能读取处理Excel等文件。

pandas最著名的就是DataFrame了。

#### Matplotlib

一个很著名的画图的库

#### Shapely

一个几何库

#### scikit-opt

这个库看起来很有用，是一个封装了若干个算法的库，专用于做规划。看起来很有用。

### 规划问题

线性规划有很多工具可以使用。

#### 使用MATLAB解决规划问题

##### 基于问题的建模

对于一个问题，MATLAB目前提供了“基于问题的线性规划”工具，可以直接录入实际问题、使用现有的模型进行计算。

步骤基本如下：

- 使用 `optimproblem` 创建一个优化问题，或使用 `eqnproblem` 创建一个方程求解问题。
- 使用 `optimvar` 创建优化变量。
- 使用表示目标、约束或方程的优化变量创建表达式。
- 对于非线性问题，创建一个初始点 `x0` 作为结构体，将优化变量的名称作为字段。
- 通过调用 [`solve`](https://ww2.mathworks.cn/help/optim/ug/optim.problemdef.optimizationproblem.solve.html) 求解问题。

一个举例：

[混合整数线性规划基础：基于问题 - MATLAB & Simulink - MathWorks 中国](https://ww2.mathworks.cn/help/optim/ug/mixed-integer-linear-programming-basics-problem-based.html)

<!--iframe src="https://ww2.mathworks.cn/help/optim/ug/mixed-integer-linear-programming-basics-problem-based.html" width="100%" height="1000px"></iframe-->
#### 基于模型的规划问题建模

这是更加传统的思路，即选择模型，然后规划建模。这个思路相对普适，但是不再像上文那样开箱即用，那么方便。

规划分为线性规划和非线性规划。线性规划是比较经典的规划模型，相对直观且简单。如果可以通过合理的数学推导，将一个相对复杂的关系，转换为一个线性规划问题，将会大大简化数学模型。

（未完待续）

