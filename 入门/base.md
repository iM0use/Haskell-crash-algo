#! https://zhuanlan.zhihu.com/p/488707305
# Haskell (lambda) 入门


> 纯属虚构

```console
im0use@im0use-virtual-machine:~$ ghci
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
Prelude>
Prelude> :set prompt "ghci>"
ghci>
```

对于函数的应用，大概框架

`fun arg1 arg2...`将函数名放在前面，后面写出参数，用space隔开

```console
ghci>div 91 10
9
ghci>91 / 10
9.1
ghci>mod 91 10
1
```

函数嵌套使用，类似复合函数

```console
ghci>succ (succ 3)
5
```

通过单变量复合函数，整出一个双参数符合函数。

比如求三个数中最大的数字

```
threeMx x y z = max (max x y) z
```

写一个`baby.hs`保存起来。

```Haskell
doubleMe x = x + x
```

```console
ghci>:l baby
[1 of 1] Compiling Main             ( baby.hs, interpreted )
Ok, modules loaded: Main.
ghci>doubleMe 4
8
```

if-else

有if就有else

对比c

```Haskell
doubleSmallNumber x = if x > 100 then x else x*2
```

```c
int doubleSmallNumber(int x){
    if(x>100){
        return x;
    } else {
        return x*2;
    }
}
```

我一次见过函数名上可以带单引号(')的，话说看着物理解题经常用，用Haskell的可以自称"理科生"吧 (自信.jpg)。

- 通常使用单引号表示函数的不懒惰版本或者小修版本。

同时具有C语言所不具备的功能，比如下面这个代码

```Haskell
doubleSmallNumber x = (if x > 100 then x else x*2)+1
```

意味着if-else语句可以作为一个表达式，就比如`x = x + 1`中的`x + 1`可以赋给左边变量一个返回值。

同样的Python也引进了lambda 即 $\lambda$ 语法

```python
In [2]: doubleSmallNumber = lambda x : x if x > 100 else x*2

In [3]: doubleSmallNumber(30)
Out[3]: 60

In [4]: doubleSmallNumber(100)
Out[4]: 200

In [5]: doubleSmallNumber(101)
Out[5]: 101
```

`encourge = "good job!"`如此定义一个函数，使用时不需要给定参数，可以称为一个定义。

```console
d**k>let encourge = "good job!"
d**k>encourge == "good job!"
True
```

文档上说"strings are lists"，所以就有

```console
d**k>let hello = ['h','e','l','l','o']
d**k>hello
"hello"
```

列表和字符串拼接用`++`。

将元素放入列表的开头很快

```console
gchi>1:[2,3]
[1,2,3]
gchi>'A':"1nice"
"A1nice"
```

因此列表可以有一个集合和元素进行推断，但集合并不是真正意义的集合。我们可以得出关系式`[1,2,3] == 1:2:3:[]`。这是正确的。

```console
ghci>[]:[]
[[]]
ghci>[] == []:[]
False
ghci>[[]:[]]
[[[]]]
```

表面上这些列表里没有元素，实际上是有的，它包含一个空的列表。

`!!`: 访问列表元素

```console
ghci>let arr = ['1','2']
ghci>arr !! 2
*** Exception: Prelude.!!: index too large
ghci>arr !! 1
'2'
ghci>arr !! 0
'1'
```

`<`,`<=`,`>`,`>=`可以用于比较列表和字符串。规则：字典序

```console
ghci>"sage" > "mathic"
True
ghci>[4,2] > [3,2,1]
True
ghci>[2,4] == [2,4]
True
```

其他列表函数

```console
ghci>let xun = [1..5]
ghci>xun
[1,2,3,4,5]
ghci>head xun
1
ghci>tail xun
[2,3,4,5]
ghci>last xun
5
ghci>init xun
[1,2,3,4]
```

- tail：除去head元素的列表

- init：返回一个列表除了最后一个元素

- 不要对空列表使用这些函数

可以通过一些函数判断是不是空列表，[[]]不是空列表，因为空列表的长度是0，[[]]的长度为1。

```console
ghci>xun
[1,2,3,4,5]
ghci>length xun
5
ghci>null xun
False
ghci>null []
True
ghci>null [[]]
False
ghci>reverse xun
[5,4,3,2,1]
```

`take`: 从列表中取元素

```console
ghci>xun
[1,2,3,4,5]
ghci>take 3 xun
[1,2,3]
ghci>take 0 xun
[]
ghci>take 8 xun
[1,2,3,4,5]
```

- `drop`: 去掉列表前几个元素的剩余列表

- `minimum`: 列表最小的元素

- `maximum`: 列表最大的元素

```
ghci>drop 3 xun
[4,5]
ghci>drop 0 xun
[1,2,3,4,5]
ghci>drop 8 xun
[]
ghci>minimum xun
1
ghci>maximum xun
5
```

当然还少不了`sum`，`product`。

elem: 成分。

> 看看你是什么东西

```console
ghci>9 `elem` [4, 9, 8]
True
```

```console
ghci>[1..10]
[1,2,3,4,5,6,7,8,9,10]
ghci>['a'..'z']
"abcdefghijklmnopqrstuvwxyz"
```

等价于`range(1,11)`

```console
ghci>[2,4..9]
[2,4,6,8]
```

依据前两个元素，进行线性推算。

