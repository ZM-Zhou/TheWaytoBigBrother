关于python编程建议的备忘
===
以下内容大部分摘自 Weiting Solid Python Code: 91 Suggestions to Improve Your Python Program

### == 与 is 的区别
`==`在python是比较两个对象的值是否相等，`is`用来检查对象标识符是否相等。对于字符串来说，对于短字符串，python的字符串驻留机制会使其拥有相同的对象标识符，所以用`is`检查，两个短字符串可以返回`True`，而长字符串没有这个特性。<br>
from Sug.16

### 擅用 with 来关闭资源
在需要打开json或txt文件进行读写的时候，优先使用`with`来进行操作，防止文件没有被正确关闭而导致问题。使用的格式如下
```
with open("file.txt", "w") as f:
    f.write("test")
```
from Sug.22

### 使用 else 判断循环退出
一般来说，判断循环是否退出，用其他语言逻辑进行编写采用如下方式：
```
found_flag = False
for i in num_list:
    if i == req_num:
        found == True
        break
    
if found_flag == False:
    print("{req_num} is not found")
```
在Python中，可以使用`else`来进行跳出的判断，对于循环自然结束，`for`之后的`else`会被执行一遍，利用这个性质可以进行相关的判断，实现如下所示：
```
for i in num_list:
    if i == req_num:
        found == True
        break
else:
    print("{req_num} is not found")
```
相似的操作还可以用在`try`语句中。<br>
from Sug.23

### 关于None
`None`在Python是一个特殊的类型，在以下情况会出现数据被视为`None`：<br>
1.常量`None`；<br>
2.常量`Fasle`；<br>
3.任何形式的数值类型`0`；<br>
4.空的序列`''`, `()`, `[]`；<br>
5.空的字典`{}`；<br>
6.用户定义了`nonzero()`, `len()`方法的是返回`0`或`False`的时候。<br>
其中常量`None`又是比较特殊的，其只有在和`None`进行比较的时候才会返回真，其他例如`list is None`, `list == None`都将返回False。所以如果需要进行空列表或空字典的判断，需要用以下格式，调用内部方法（情况6）来进行判断。
```
if list:                 #value is not empty
    Do something
else:                    #value is empty
    Do some other thing
```
from Sug.26

### 大规模的字符串连接优先使用join方法
在进行大规模的字符串连接时，受Python运算方式的影响，应该优先使用`join`方法，其效率比直接用`+`要高得多。join的用法如下：<br>
`"".join([str1, str2, str3])`<br>
from Sug.27

### 字符串格式化方法 .format()
Python的字符串格式化方法`.fotmat()`相当灵活和方便。在深度学习训练过程输出的时候可以起到相当大的作用。下面是几个常用格式的用法：
```
line = "time: {time} | proces_num: {num:0>10} | total_loss: {loss:S^15.3e} | "\
    .format(time="10h10m", num=20, loss=0.9876)
# 输出结果
# time: 10h10m | proces_num: 0000000020 | total_loss: SSS9.876e-01SSS |
```
其中在字符串中的占位符的基本语法如下：<br>
`{[变量名]:[[填充符]对齐方式][符号][#][0][宽度][,][.精确度][转换类型]}`<br>
变量名、宽度和精确度与C语言的格式化相似，变量名可以是除了花括号的仍和字符，如上例中的0和S。对齐方式有`<``>``^`三种，分别左对齐，右对齐和居中。符号有`+``-`` `分别正负前面都有符号、只有负数前面有负号和正数前面加空格，负数前面加负号。<br>
from Sug.28

### 可变对象/不可变对象与参数传递
Python中的对象分为可变对象（字典、列表和字节数组）与不可变对象（数字、字符串和元组）。其在使用上是有一定区别的。首先需要了解的是Python的对象实际是一个对象指针，这个指针指向存储相应内容的内存，而赋值将创建一个新的指针指向同一个内存。<br>
在进行可变对象操作的时候，对新的对象进行操作，原本的对象也会改变。需要注意的是，切片操作相当于浅拷贝，会开辟一片新的内存空间，所以修改切片创建的可变对象，不会对原来的对象产生影响。具体可以参考下面的例子。
```
list1 = ["a", "b", "c"]
list2 = list1
list3 = list1[:]
list2.append("d")
list3.remove("a")
print(list1)
print(list2)
print(list3)
print(id(list1))
print(id(list2))
print(id(list3))
# 输出结果
# ['a', 'b', 'c', 'd']
# ['a', 'b', 'c', 'd']
# ['b', 'c']
# 2005239757640
# 2005239757640
# 2005239556424
```
还要注意的是，在Python创建类的时候，其`__init__()`函数若存在默认参数，只会被评估一次，之后每次实例化都会使用这个参数，这时如果使用了`[]`作为默认参数传入，在之后每次实例化的时候，都会指向这片内存，导致错误。所以在`__init()__`中，应该尽量使用`None`作为默认参数，在每次实例化的时候动态生成。

对于不可变对象，由于对象的更新相当于指针指向一片新的内存，所以不可变对象之间不会产生影响。具体可见下面的例子
```
str1 = "hellow world"
str2 = str1
str2 = str2[:6]
print(str1)
print(str2)
# 输出结果
# hellow world
# hellow
```
理解了上述原理，对于Python函数的参数传递也就好理解了。参数传递的本质是传递对象，或者说是对象的引用。所以表现出的性质是可变对象，在函数内外共享同一片内存，在函数内的操作是函数外可见的。而对于不可变对象，赋值相当于创建了一个同名局部变量，并指向新的内存，所以函数外对不可变对象的操作是无法看到的。<br>
from Sug.29 Sug.31

### 列表，字典和元组的初始化
在Python中，相比使用循环来初始化一个列表，使用列表解析是更推荐的方式，其一般格式如下<br>
`[表达式 for 迭代变量 in 可迭代变量 if 条件表达式]`<br>
解析将遍历可迭代变量中的值，若符合条件表达式则加入列表中，最后返回列表。列表解析支持多重迭代（相当于多个循环嵌套），其中表达式也可以是一个简单函数，而可迭代变量也支持对文件的操作，下面是一个具体的例子。
```
def my_fun(n):
    if n % 2 == 0:
        n = n ** 2
    else:
        n = n + 1
    
    return n

list1 = [my_fun(i) + j for i in [2, 3, 6, -1] for j in [1, 3, 4, 7] if i > 0 and j % 2]
print(list1)

# 输出结果
# [5, 7, 11, 5, 7, 11, 37, 39, 43]
```
from Sug.30

### 区分浅拷贝和深拷贝
在Python中进行符合对象的拷贝的时候，可以使用Python自带的`copy`模块来进行。需要区分的是，拷贝分为浅拷贝和深拷贝两种，分别对应`copy`模块中的`copy.copy()`和`copy.deepcopy()`。其区别在于浅拷贝只会拷贝引用，而深拷贝会拷贝复合对象中的具体内容。<br>
具体例子来说，如果拷贝对象中存在可变对象，比如一个列表，若使用浅拷贝，则修改拷贝中的数组，原对象中的数组也会跟着改变，因为其内容所指的id是相同的。在使用中要注意这样的情况<br>
from Sug.38