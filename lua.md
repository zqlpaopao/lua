https://www.runoob.com/lua/lua-environment.html

# 安装

```
curl -R -O http://www.lua.org/ftp/lua-5.3.0.tar.gz
tar zxf lua-5.3.0.tar.gz
cd lua-5.3.0
make macosx test
make install
```

# 注视

单行注视 --

多行注视

--[[xxxxxx--]]

![image-20200714220916243](lua.assets/image-20200714220916243.png)



# 变量命名规则

- 标示符以一个字母 A 到 Z 或 a 到 z 或下划线 _ 开头后加上0个或多个字母，下划线，数字（0到9）
- 不能数字开头
- 不推荐_开头
- 区分大小写
- 不建议使用关键词

## 关键词

以下列出了 Lua 的保留关键字。保留关键字不能作为常量或变量或其他用户自定义标示符：

| and      | break | do    | else   |
| -------- | ----- | ----- | ------ |
| elseif   | end   | false | for    |
| function | if    | in    | local  |
| nil      | not   | or    | repeat |
| return   | then  | true  | until  |
| while    | goto  |       |        |

一般约定，以下划线开头连接一串大写字母的名字（比如 _VERSION）被保留用于 Lua 内部全局变量。

## 全局变量

在默认情况下，变量总是认为是全局的。

全局变量不需要声明，给一个变量赋值后即创建了这个全局变量，访问一个没有初始化的全局变量也不会出错，只不过得到的结果是：nil。

\> print(b)
nil
\> b=10
\> print(b)
10
\>

<font color=red>如果你想删除一个全局变量，只需要将变量赋值为nil。</font>

```
b = nil
print(b)      --> nil
```

这样变量b就好像从没被使用过一样。换句话说, 当且仅当一个变量不等于nil时，这个变量即存在。



# Lua 数据类型

Lua 是动态类型语言，变量不要类型定义,只需要为变量赋值。 值可以存储在变量中，作为参数传递或结果返回。

Lua 中有 8 个基本类型分别为：nil、boolean、number、string、userdata、function、thread 和 table。

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| nil      | 这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false）。 |
| boolean  | 包含两个值：false和true。                                    |
| number   | 表示双精度类型的实浮点数                                     |
| string   | **字符串由一对双引号或单引号来表示**                         |
| function | 由 C 或 Lua 编写的函数                                       |
| userdata | 表示任意存储在变量中的C数据结构                              |
| thread   | 表示执行的独立线路，用于执行协同程序                         |
| table    | Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字、字符串或表类型。在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。 |

我们可以使用 type 函数测试给定变量或者值的类型：

## 实例

```
print(type("Hello world"))    *--> string*
print(type(10.4*3))       *--> number*
print(type(print))        *--> function*
print(type(type))        *--> function*
print(type(true))        *--> boolean*
print(type(nil))         *--> nil*
print(type(type(X)))       *--> string*
```

![image-20200714222130784](lua.assets/image-20200714222130784.png)

------

## nil（空）

nil 类型表示一种没有任何有效值，它只有一个值 -- nil，例如打印一个没有赋值的变量，便会输出一个 nil 值：

```
> print(type(a))
nil
>
```

==对于全局变量和 table，nil 还有一个"删除"作用==，给全局变量或者 table 表里的变量赋一个 nil 值，等同于把它们删掉，执行下面代码就知：

tab1 = { key1 = "val1", key2 = "val2", "val3" }
**for** k, v **in** pairs(tab1) **do**
  print(k .. " - " .. v)
**end**

tab1.key1 = nil
**for** k, v **in** pairs(tab1) **do**
  print(k .. " - " .. v)
**end**

### nil 作比较时应该加上双引号 **"**：

```
\> type(X)
nil
\> type(X)==nil
false
\> type(X)=="nil"
true
\>
```



**type(X)==nil** 结果为 **false** 的原因是因为 **type(type(X))==string**。

------

## boolean（布尔）

==boolean 类型只有两个可选值：true（真） 和 false（假），Lua 把 false 和 nil 看作是 false，其他的都为 true，数字 0 也是 true:==

## 实例

```
print(type(true))
print(type(false))
print(type(nil))

if false or nil then
  print("至少有一个是 true")
else
  print("false 和 nil 都为 false")
end

if 0 then
  print("数字 0 是 true")
else
  print("数字 0 为 false")
end
```



以上代码执行结果如下：

```
$ lua test.lua 
boolean
boolean
nil
false 和 nil 都为 false
数字 0 是 true
```

------

## number（数字）

Lua 默认只有一种 number 类型 -- double（双精度）类型（默认类型可以修改 luaconf.h 里的定义），以下几种写法都被看作是 number 类型：

## 实例

```
print(type(2))
print(type(2.2))
print(type(0.2))
print(type(2e+1))
print(type(0.2e-1))
print(type(7.8263692594256e-06))

```



[运行实例 »](https://www.runoob.com/try/runcode.php?filename=datatype1&type=lua)

以上代码执行结果：

```
number
number
number
number
number
number
```

------

## string（字符串）

==字符串由一对双引号或单引号来表示。==

```
string1 = "this is string1"
string2 = 'this is string2'
```

也可以用 2 个方括号 "[[]]" 来表示"一块"字符串。

## 实例

```
html = [[
<html>
<head></head>
<body>
    <a href="http://www.runoob.com/">菜鸟教程</a>
</body>
</html>
]]
print(html)
```

以下代码执行结果为：

```
<html>
<head></head>
<body>
    <a href="http://www.runoob.com/">菜鸟教程</a>
</body>
</html>
```

==在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字:==

\> print("2" + 6)
8.0
\> print("2" + "6")
8.0
\> print("2 + 6")
2 + 6
\> print("-2e2" * "6")
-1200.0
\> print("error" + 1)
stdin:1: attempt to perform arithmetic on a string value
stack traceback:
    stdin:1: **in** main chunk
    [C]: **in** ?
\>

以上代码中"error" + 1执行报错了，==字符串连接使用的是 .. ==，如：

```
> print("a" .. 'b')
ab
> print(157 .. 428)
157428
> 
```

==使用 # 来计算字符串的长度==，放在字符串前面，如下实例：

## 实例

```
\> len = "www.runoob.com"
\> print(#len)
14
\> print(#"www.runoob.com")
14
\>
```



------

## table（表）

在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。也可以在表里添加一些数据，直接初始化表:

### 实例

```
*-- 创建一个空的 table*
**local** tbl1 = {}
 
*-- 直接初始表*
**local** tbl2 = {"apple", "pear", "orange", "grape"}

Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。
```

### 实例

```
*-- table_test.lua 脚本文件*
a = {}
a["key"] = "value"
key = 10
a[key] = 22
a[key] = a[key] + 11
**for** k, v **in** pairs(a) **do**
  print(k .. " : " .. v)
**end**
```



脚本执行结果为：

```
$ lua table_test.lua 
key : value
10 : 33
```

![image-20200714223225688](lua.assets/image-20200714223225688.png)

### 实例

不同于其他语言的数组把 0 作为数组的初始索引，在 Lua 里表的默认初始索引一般以 1 开始。

```
-- table_test2.lua 脚本文件*
local tbl = {"apple", "pear", "orange", "grape"}
for key, val in pairs(tbl) do
  print("Key", key)
end
```



脚本执行结果为：

```
$ lua table_test2.lua 
Key    1
Key    2
Key    3
Key    4
```

==table 不会固定长度大小，有新数据添加时 table 长度会自动增长，没初始的 table 都是 nil。==

### 实例

```
-- table_test3.lua 脚本文件*
a3 = {}
for i = 1, 10 do
  a3[i] = i
end
a3["key"] = "val"
print(a3["key"])
print(a3["none"])
print(a3[1])
print(a3[5])
```



脚本执行结果为：

```
$ lua table_test3.lua 
val
nil
```

-- table_test3.lua 脚本文件*
a3 = {}
for i = 1, 10 do
  a3[i] = i
end
a3["key"] = "val"
print(a3["key"])
print(a3["none"])
print(a3[1])
print(a3[5])![image-20200714223448150](lua.assets/image-20200714223448150.png)

------



## function（函数）

在 Lua 中，函数是被看作是"第一类值（First-Class Value）"，函数可以存在变量里:

### 实例

```
-- function_test.lua 脚本文件*
function factorial1(n)
  if n == 0 then
    return 1
  else
    return n * factorial1(n - 1)
  end
end
print(factorial1(5))
factorial2 = factorial1
print(factorial2(5))
```



脚本执行结果为：

```
$ lua function_test.lua 
120
120
```

function 可以以匿名函数（anonymous function）的方式通过参数传递:

![image-20200714223828148](lua.assets/image-20200714223828148.png)

### 实例

```
-- function_test2.lua 脚本文件*
function testFun(tab,fun)
    for k ,v in pairs(tab) do
        print(fun(k,v));
    end
end


tab={key1="val1",key2="val2"};
testFun(tab,
function(key,val) --匿名函数
    return key.."="..val;
end
);
```



脚本执行结果为：

```
$ lua function_test2.lua 
key1 = val1
key2 = val2
```

![image-20200714224118009](lua.assets/image-20200714224118009.png)





## thread（线程）

在 Lua 里，最主要的线程是协同程序（coroutine）。它跟线程（thread）差不多，拥有自己独立的栈、局部变量和指令指针，可以跟其他协同程序共享全局变量和其他大部分东西。

线程跟协程的区别：线程可以同时多个运行，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起（suspend）时才会暂停。

------

## userdata（自定义类型）

userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用。



# Lua 变量

==变量在使用前，必须在代码中进行声明，即创建该变量。==

编译程序执行代码之前编译器需要知道如何给语句变量开辟存储区，用于存储变量的值。

Lua 变量有三种类型：全局变量、局部变量、表中的域。

<font color = red>Lua 中的变量全是全局变量，那怕是语句块或是函数里，除非用 local 显式声明为局部变量。</font>

局部变量的作用域为从声明位置开始到所在语句块结束。

**变量的默认值均为 nil。**

### 实例

```
-- test.lua 文件脚本*
a = 5        -- 全局变量*
local b = 5     -- 局部变量*

function joke()
  c = 5      -- 全局变量*
  local d = 6   -- 局部变量*
end

joke()
print(c,d)      --> 5 nil*

do
  local a = 6   -- 局部变量*
  b = 6      -- 对局部变量重新赋值*
  print(a,b);   --> 6 6*
end

print(a,b)    --> 5 6*
```

执行以上实例输出结果为：

```
$ lua test.lua 
5    nil
6    6
5    6
```

![image-20200714224712397](lua.assets/image-20200714224712397.png)



## 赋值语句

赋值是改变一个变量的值和改变表域的最基本的方法。

```
a = "hello" .. "world"
t.n = t.n + 1
```

Lua 可以对多个变量同时赋值，变量列表和值列表的各个元素用逗号分开，赋值语句右边的值会依次赋给左边的变量。

```
a, b = 10, 2*x       <-->       a=10; b=2*x
```

遇到赋值语句Lua会先计算右边所有的值然后再执行赋值操作，所以我们可以这样进行交换变量的值：

```
x, y = y, x                     -- swap 'x' for 'y'
a[i], a[j] = a[j], a[i]         -- swap 'a[i]' for 'a[j]'
```

当变量个数和值的个数不一致时，Lua会一直以变量个数为基础采取以下策略：

```
a. 变量个数 > 值的个数             按变量个数补足nil
b. 变量个数 < 值的个数             多余的值会被忽略
```

## 实例

```lua
a, b, c = 0, 1
print(a,b,c)       --> 0  1  nil*

a, b = a+1, b+1, b+2   -- value of b+2 is ignored*
print(a,b)        --> 1  2*

a, b, c = 0
print(a,b,c)       --> 0  nil  nil*
```

上面最后一个例子是一个常见的错误情况，注意：如果要对多个变量赋值必须依次对每个变量赋值。

```
a, b, c = 0, 0, 0
print(a,b,c)             --> 0   0   0
```

![image-20200714225414237](lua.assets/image-20200714225414237.png)

多值赋值经常用来交换变量，或将函数调用返回给变量：

```
a, b = f()
```

f()返回两个值，第一个赋给a，第二个赋给b。

应该尽可能的使用局部变量，有两个好处：

- 1. 避免命名冲突。
- 2. 访问局部变量的速度比全局变量更快。

------

## 索引

对 table 的索引使用方括号 []。Lua 也提供了 . 操作。

```
t[i]
t.i                 -- 当索引为字符串类型时的一种简化写法
gettable_event(t,i) -- 采用索引访问本质上是一个类似这样的函数调用
```

## 实例

```
 site = {}
 site["key"] = "www.runoob.com"
 site[2] = "www.222.com"
 print(site["key"])
--www.runoob.com
 print(site.key)
 --print(site.2) 数字是不可以的
--www.runoob.com
```



![image-20200714225748817](lua.assets/image-20200714225748817.png)

































