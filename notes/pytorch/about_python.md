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

### 关于plt的图片加载
```
import matplotlib.image as mpimg # mpimg 用于读取图片
img = mpimg.imread('image/path') # 读取图片为np array格式
```
