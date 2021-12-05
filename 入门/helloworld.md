# 函数式编程初探



## install

Haskell 的初步安装：Linux Ubuntu平台，指令为`$ sudo apt-get install haskell-platform `



## 编程环境

在Haskell语言的众多实现中，有两个被广泛应用，**Hugs** 和 **GHC **。

- 其中 **Hugs** 是一个解析器，主要用于教学。
- 而GHC(Glasgow Haskell Compiler)更加注重实践，它编译成本地代码，支持并行执行，并带有更好的性能分析工具和调试工具。由于这些因素，在本书中我们将采用 ***GHC*** 。

GHC主要有三个部分组成。

- **ghc **是生成快速本底代码的优化编译器。
- **ghci **是一个交互解析器和调试器。
- **runghc** 是一个以脚本形式(并不要首先编译)运行Haskell代码的程序，

在类Unix系统中，我们在shell视窗下运行**ghci**。

```shell
$ ghci
GHCi, version 6.8.3: http://www.haskell.org/ghc/  :? for help
Loading package base ... linking ... done.
Prelude>
```

提示符`Prelude`标识一个很常用的库`Prelude`已经被加载并可以使用。同样的，当加载了其他模块或是源文件时，它们也会在出现在提示符的位子。

- Tip

    - 获取帮助信息
    - 在ghci提示符输入 :?，则会显示详细的帮助信息。

模块`Prelude`有时候被称为“标准序幕”(the standard prelude)，因为它的内容是基于Haskell 98标准定义的。通常简称它为“序幕”(the prelude)。

### Note

关于ghci的提示符

提示符经常是随着模块的加载而变化。因此经常会变得很长以至在单行中没有太多可视区域用来输入。

为了简单和一致起见，在本书中我们会用字符串 ‘ghci>’ 来替代ghci的默认提示符。

你可以用ghci的 `:set prompt` 来进行修改。

```
Prelude> :set prompt "ghci>"
ghci>
```

### 简易计算器

此部分了解一下

### 提示

```haskell
ghci> :info (+)
class (Eq a, Show a) => Num a where
  (+) :: a -> a -> a
  ...
    -- Defined in GHC.Num
infixl 6 +
ghci> :info (*)
class (Eq a, Show a) => Num a where
  ...
  (*) :: a -> a -> a
  ...
    -- Defined in GHC.Num
infixl 7 *
```

### 未定义的变量以及定义变量

Haskell的标准库prelude定义了至少一个大家熟知的数学常量。

```haskell
ghci> pi
3.141592653589793
```

然后我们很快就会发现它对数学常量的覆盖并不是很广泛。让我们来看下Euler数，`e`。

```haskell
ghci> e

<interactive>:1:0: Not in scope: `e'
```

啊哈，看上去我们必须得自己定义。

```
不要担心错误信息
以上“not in the scope”的错误信息看上去有点令人畏惧的。别担心，它所要表达的只是没有用e这个名字定义过变量。
```

使用**ghci**的`let`构造器(contruct)，我们可以定义一个临时变量e。

```haskell
ghci> let e = exp 1
```

这是指数函数`exp`的一个应用，也是如何调用一个Haskell函数的第一个例子。 像Python这些语言，函数的参数是位于括号内的，但**Haskell不要那样**。

既然`e`已经定义好了，我们就可以在数学表达式中使用它。我们之前用到的乘方操作符`(^)`是对于整数的。如果要用浮点数作为指数，则需要操作符`(**)`。

### Note

这是ghci的特殊语法

ghci 中 `let` 的语法和常规的“top level”的Haskell程序的使用不太一样。我们会在章节“初识类型”里看到常规的语法形式。

### 处理优先级以及结合性规则

有时候最好显式地加入一些括号，即使Haskell允许省略。它们会帮助将来的读者，包括我们自己，更好的理解代码的意图。

更加重要的，基于操作符优先级的复杂的表达式经常引发bug。对于一个简单的、没有括号的表达式，编译器和人总是很容易的对其意图产生不同的理解。

不需要去记住所有优先级和结合性规则：在你不确定的时候，加括号是最简单的方法。

## ghci里的命令行编辑

在大多数系统中，**ghci**有些命令行编辑的功能。如果你对命令行编辑还不熟悉，它将会帮你节省大量的时间。 基本操作对于类Unix系统和Windows系统都很常规。按下**向上**方向键会显示你输入的上一条命令；重复输入**向上**方向键则会找到更早的一些输入。可以使用**向左**和**向右**方向键在当前行移动。 在类Unix系统中(很不幸，不是Windows)，**制表键**(tab)可以完成输入了一部分的标示符。

[译者注：]制表符的完成功能其实在Windows下也是可以的。

Tip

哪里可以找到更多信息

我们只是蜻蜓点水般的介绍了下命令行编辑功能。因为命令行编辑系统可以让你更加有效的工作，你可能会觉得进一步的学习会有帮助。

在类Unix系统下，**ghci**使用功能强大并且可定制化的[GNU readline library](http://tiswww.case.edu/php/chet/readline/rltop.html#Documentation)。在Windows系统下，**ghci**的命令行编辑功能是由[doskey command](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/doskey.mspx)提供的。

## 列表(Lists)

一个列表由方括号以及被逗号分隔的元素组成。

```haskell
ghci> [1, 2, 3]
[1,2,3]
```

### Note

逗号是分隔符，不是终结符

有些语言在表示列表时会在右中括号前多一个逗号，但是Haskell没有这样做。如果多出一个逗号(比如 `[1,2,]` )，则会导致编译错误。

列表可以是任意长度。空列表表示成`[]`。

```haskell
ghci> []
[]
ghci> ["foo", "bar", "baz", "quux", "fnord", "xyzzy"]
["foo","bar","baz","quux","fnord","xyzzy"]
```

列表里所有的元素必须是相同类型。下面例子我们违反了这个规则：列表中前面两个是`Bool`类型，最后一个是字符类型。

```haskell
ghci> [True, False, "testing"]

<interactive>:1:14:
    Couldn't match expected type `Bool' against inferred type `[Char]'
      Expected type: Bool
      Inferred type: [Char]
    In the expression: "testing"
    In the expression: [True, False, "testing"]
```

这次**ghci**的错误信息也是同样的很详细。它告诉我们无法把字符串转换为布尔类型，因此无法定义这个列表表达式的类型。

如果用*列举符号(enumeration notation)*来表示一系列元素，Haskell则会自动填充内容。

```haskell
ghci> [1..10]
[1,2,3,4,5,6,7,8,9,10]
```

