---
layout:     post
title:      "Python 入门"
subtitle:   "Python 入门"
date:       2016-06-14
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - python
---

## 注释

~~~ python
# 这是注释
~~~

## 算数运算符
* +
* -
* *
* /
* %
* //	浮点除法（对结果四舍五入）
* **	乘方

~~~ python
>>> 10 / 1.2
8.33333333333333...
>>> 10 // 1.2
8
>>> 2 ** 2	#(2的平方)
4
>>> 2 ** 3	#(2的3次方)
8
~~~

## 赋值运算符
* =
* +=
* -=
* /=
* *=

## 比较运算符
* <
* <=
* .>
* .>=
* ==
* !=
* <>

## 逻辑运算符
* and
* or
* not

## ++，--支持
不支持

## 变量
~~~
counter = 0
miles = 1000.0
name = 'Bob'
~~~

## 数据类型
* int
* long
* bool
* float
* decimal
* complex

## 字符串
~~~ python
>>> pystr = 'python'
>>> iscool = "is cool!"
>>> pystr[0]
'p'
>>> pystr[2:5]
'tho'
>>> iscool[:2]
'is'
>>> iscool[3:]
'cool!'
>>> iscool[-1]
'!'
>>> pystr + iscool
'Pythonis cool!'
>>> pystr + ' ' + iscool
'Python is cool!'
>>> pystr * 2
'PythonPython'
>>> pystr = '''python
... is cool'''
>>> pystr
'python\nis cool'
>>> print pystr
python
is cool
~~~

## 三元表达式

## 数组

## 列表
列表元素用[]包裹，元素的个数及元素的值可以改变.

~~~ python
>>> aList = [1, 2, 3, 4]
>>> aList
[1, 2, 3, 4]
>>> aList[0]
1
>>> aList[2:]
[3, 4]
>>> aList[1] = 5
>>> aList
[1, 5, 3, 4]
~~~

## 元组
元组元素用()包裹，元素的值无法更改, 变量本身可以重新被赋值.

~~~ python
>>> aTuple = ('robots', 77, 93, 'try')
>>> aTuple
('robots', 77, 93, 'try')
>>> aTuple[:3]
('robots', 77, 93)
>>> aTuple[1] = 5
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> aTuple = ('a', 1, 2, 3)
>>> aTuple
('a', 1, 2, 3)
~~~

## 字典
字典元素用{}包裹.

~~~ python
>>> aDict = {'host': 'earth'}
>>> aDict['port'] = 80
>>> aDict
{'host': 'earth', 'port': 80}
>>> aDict.keys()
['host', 'port']
>>> aDict['host']
earth
>>> for key in aDict:
...   print key, aDict[key]
...
host earth
port 80
~~~

## 条件控制语句

~~~ python
n = 3
if n > 2:
  print "n > 2"

if n < 2:
  print "if"
else
	print "else"

if n < 2:
  print "if"
elif n == 3:
	print "n is 3"
else
	print "else"
~~~

## 循环语句

~~~ python
i = 0
while i < 3:
	print "#%d"

for item in [1, 2, 3, 4]:
	print item

foo = "abc"
for i in range(len(foo)):
	print foo[i], "(%d)" % i

~~~

## 异常

~~~ python
try:
    fileName = raw_input("please input file name:")
    file = open(fileName, 'r')
    for line in file:
        print line
    file.close()
except IOError, e:
	print "file open error", e

raise Exception("message") # 语句可以引发一个异常, 类似Java的throw
~~~

## Python 特性

### 列表解析

~~~ python
>>> squred = [x ** 2 for x in range(4)]
>>> for i in squred:
... 	print i
...
0
1
4
9

>>> squred = [x ** 2 for x in range(8) if not x % 2]
>>> for i in squred:
... 	print i
...
0
4
16
36
~~~

### 函数
* 定义函数

~~~ python
def funtion_name([arguments]):
    "optional documentation string"
    function suite
~~~
* 函数的参数可以有带默认值

~~~ python
def foo(debug=True):
    "optional documentation string"
    if debug:
        print "xxx"
~~~

### 类
使用class定义类，可以提供一个可选的父类或者说基类，如果没有合适的基类，那就使用object作为基类。class行之后是可选的文档字符串，静态成员定义，方法定义。

#### 定义类

~~~ python
class ClassName(base_class[es]):
    "optional document string"
    static_member_declarations
    method_declarations
~~~

#### 示例

~~~ python
class FooClass(object):
    "my first class"
    version = 0.1

    def __init__(self, nm='john doe'):
        "constructor"
        self.name = nm
        print "created a class instance for", nm

    def showname(self):
        "display instance attribute and class name"
        print "Your name is ", self.name
        print "My Name is ", self.__class__.__name__

    def showver(self):
        "display class(static) attribute"
        print self.version

    def addMe2Me(self, x):
        "apply + operation to argument"
        return x + x
~~~

#### 测试

~~~ python
>>> foo1 >>> = FooClass()
created a class instance for john doe
>>> foo2 = FooClass("Ely")
created a class instance for Ely
>>> foo2.showname()
Your name is  Ely
My Name is  FooClass
>>> foo2.showver()
0.1
>>> print foo2.addMe2Me(2)
4
~~~

### 模块
模块是一种组织形式，它将彼此有关系的python代码组织到一个个独立文件当中。
模块可以包含可执行代码，函数和类或者这些东西的组合。

**当你创建了一个Python源文件，模块的名字就是不带 .py后缀的文件名。 一个模块创建之后，你可以从另一个模块中使用import语句导入这个模块来使用。**

* 导入模块
`import module_name`

* 使用模块
`module_name.functionName()`
`module_name.attributeName`

### 术语
PEB: 一个PEB就是一个Python 增强提案（Python Enhancement Proposal), 这也是在新版Python中增加新特性的方式。


### 内建函数

函数 | 描述
--------- | -------------
dir([obj]) | 显示对象的属性，如果没有提供参数，则显示全局对象的名字
help([obj]) | 以一种整齐美观的形式，显示对象的文档字符串，如果没有提供任何参数，则会进入交互式帮助
int(obj) | 将一个对象转换为一个整数
len(obj) | 返回对象的长度
open(fn, mode) | 以mode('r' = 读, 'w' = '写', 'a' = '追加')方式打开一个文件名为fn的文件
range([start,]stop[,step]) | 返回一个整数列表，起始值为start，结束值为stop - 1；start默认为0，step默认值为1
raw_input(str) | 等待用户输入一个字符串，可以提供一个可选的参数str用作提示信息
str(obj) | 将一个对象转换为字符串
type(obj) | 返回对象的类型（返回值本身是一个type对象！）
print | 打印


