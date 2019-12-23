一些常用的python操作
===
### 关于维度交换和改变
在数据操作过程中经常需要对数据的维度进行交换或者维度增加，从numpy和pytorch可以分别采用以下方法：<br>
numpy：维度交换采用`transpose()`方法，传入参数为交换后的维度顺序，例：将一个NCHW的batch转换为NHWC，则使用`transpose(0, 2, 3, 1)`。<br>
(pytorch) tensor：维度交换采用`permute()`方法，传入参数与numpy的transpose相同（需要注意的是tensor也有`transpose()`方法,但只能交换两个维度）；维度扩展采用`unsqueeze()`方法，传入参数为插入维度的编号，例：将一个CHW的图片变为1CHW的batch，则使用`unsquzzez(0)`。

### 关于数据类型转换
如果不加注意，我们会经常收到“传入参数为XXX类型，而需要XXX类型”的错误，这时候最简单的方法就是在报错点前面加一个类型转换。当然应该在编写代码的时候就时刻注意数据类型的转换。<br>
numpy：`array.astype()`<br>
tensor：`tensor.to()`，需要注意的是pytorch的to()方法不仅可以输入要转换的类型，还可以输入另一个tensor，将该tensor的类型变为与传入tensor相同的类型。

其他时候，还会需要将数据在numpy array和list之间转换。<br>
array->list：`array.tolist()`<br>
list->array：`x = np.array(x)`<br>

有时候，tuple和list之间的转换也是需要的，这里可以直接使用强制类型转换`tuple(temp_list)`或`list(temp_tuple)`

### 关于张量复制与变形
当需要对张量进行维度扩充来适应网络的输入要求，或者进行反转、复制实现某些功能的时候，在numpy和pytorch中有不同的实现方法。<br>
对于numpy：<br>
维度复制可以使用`array.repeat([], axis=n)`其传入的参数都是复制的尺寸（接收一个list或int）和重复的维度。对于一维的向量，其只能将每个元素复制n份，例如：<br>
```
import numpy as np
a = np.array([0, 1 ,2, 3, 4])
a.repeat(3)
#output
#array([0, 0, 0, 1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4])
```
对于高维向量的复制，可以使用`numpy.tile(array, (shape))`来完成，其可以直观地将某一个张量的每个维度复制相应份数，同时，还可以用这个方法来进行维度扩张，例如：<br>
```
import numpy as np
a = np.array([0, 1, 2, 3, 4])
a_r1 = np.tile(a, 3)
a_r2 = np.tile(a, (1, 3))
a_r3 = np.tile(a, (3, 1))
#output
#a_r1
#array([0, 1, 2, 3, 4, 0, 1, 2, 3, 4, 0, 1, 2, 3, 4])
#a_r2
#array([[0, 1, 2, 3, 4, 0, 1, 2, 3, 4, 0, 1, 2, 3, 4]])
#a_r3
#array([[0, 1, 2, 3, 4],
        [0, 1, 2, 3, 4],
        [0, 1, 2, 3, 4]])
```
一般来说，上述例子中a_r3是我们需要的扩张结果，`tile`方法可以直接生成，而如果使用`repeat`，则需要配合`numpy.reshape(array, (shape), order=)`来完成，例如：<br>
```
import numpy as np
a = np.array([0, 1 ,2, 3, 4])
a.repeat(3)
#output
#array([0, 0, 0, 1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4])
np.reshape(a.repeat(3), (3,5), 'F')
#output
#array([[0, 1, 2, 3, 4],
        [0, 1, 2, 3, 4],
        [0, 1, 2, 3, 4]])
```
这里的order参数可以简单理解为读取顺序，接收为`'C'`（默认）时，读取张量的顺序是最后一个维度变化最快，第一个维度变化最慢；接收`'F'`时，与之相反

参考链接：<br>
https://blog.csdn.net/yimingsilence/article/details/52924359<br>
https://blog.csdn.net/zhanggonglalala/article/details/79356653

### 关于numpy的反转操作
numpy为张量提供了方便的反转方法，分别为左右反转`numpy.fliplr(array)`和上下反转`numpy.flipud(array)`。需要注意的是，反转之后张量的存储可能出现不连续，导致对其的后续操作出现报错，这时需要调用`np.ascontiguousarray(array)`方法使其连续存储。

参考连接：<br>
https://blog.csdn.net/e01528/article/details/86067489

### 字典（dict）与元组（tuple）
python 中的字典要求键值（key）是一个可哈希的类型，除了常用的字符串和数字，元组也可以作为字典的键值。例如可以用`outputs[("disp", 0)]`来表示输出中scale值为0的视差图。需要注意的是，元组也具有可以直接相加的性质，但必须保证两侧的操作数都是元组，若想在其中加入一个元素，需要使用以下形式`tuple1 + ([elem],)`声明其是一个元组。

### 列表的查找
python提供了很方便的列表查找方法`list.index()`在index中填入要查找的元素，若列表中有这个元素，则返回其下标，否则弹出异常。同时，其还接受`start=, end=`两个可选参数，规定查找的范围。在程序中可以用如下方式使用
```
try:
    idx = my_list.index(x):
        print("{} is in {}".format(x, idx))
except:
    print("{} is not in list".format(x))
```

参考连接<br>
https://www.runoob.com/python/att-list-index.html

### 列表的排序
python提供了`list.sort()`方法对列表进行排序，其默认将列表内容按升序排序。可以传入参数`reverse=True`则将按照降序排序。有时列表中包含的是一个元祖或另一个列表，则可以通过`lambda`来指定排序的键值。考虑list中元素为("string", loss)而需要按照loss排序的情况，则可以使用以下方式：
```
list.sort(key=lambda x:x[1], reverse=True)
```

参考连接<br>
https://www.jianshu.com/p/05c113bf722d

### pyhon的面向对象
python支持面向对象的编程方法，pytorch中网络的实现，数据集的构建很多需要借助类来完成，下面讨论一点python与面向对象编程相关的内容。  
##### 参数的传递
其实这不仅是面向对象的问题，python中经常可以看到如下形式的参数传递方式：<br>
`def function(arg,*args,**kwargs):`<br>
这样的传递方式可以支持不定个数的参数传递。其中`arg`对应一个参数，也可以给其赋予有意义的名字，这与一般的参数传递是相同的。`*args`可以接收多个不带有键值的参数（即直接传入值），并以tuple形式传入函数。`**kwargs`可以接收多个带有键值的参数（即用key=value的形式传入的参数），并以dict形式传入函数。<br>
这样的参数传递方式对于下面类的继承有很大的便利性。<br>
参考链接：<br>
https://www.cnblogs.com/yunguoxiaoqiao/p/7626992.html

##### 类的继承
python的类对继承方式没有太多限制，这里暂不讨论多继承等问题，主要讨论用的较多的super()函数。<br>
一般，在继承自其他类时，会在`__init__()`方法中加入如下语句：<br>
`super([子类名], self).__init__()`<br>
这是python用于解决多重继承的机制，其不仅可用于init，也可用于其他方法。super可以自动建立方法解析顺序表，防止在多重继承，尤其是钻石继承的时候出现类的冲突。上面的使用形式是python2.x的格式，在python3.x中可以直接使用`super().__init__()`进行调用，若父类有需要传递的参数，就可以用上述提到的多参数传递来实现。

若需要对父类的方法进行重写，只需要在子类中直接对相同的方法做重新def即可，调用时默认调用子类的方法。

### python字符串引用
当python需要根据不同的情况import不同的库的时候，就可以使用字符串引用来进行动态的调用，具体的实现如下：<br>
```
from importlib import import_module
A = import_module('A')
```
之后使用 A.Class等可以实现与正常调用相同的功能。

参考连接：<br>
https://blog.csdn.net/weixin_30666401/article/details/97750378<br>
https://blog.csdn.net/edward_zcl/article/details/88809212