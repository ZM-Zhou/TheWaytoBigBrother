Pytroch的debug技巧
===
### VS Code使用jupyter notebook
在新版本的VS Code中Python已经集成了原生的Jupyter 只要在程序中输入`#%%`即可创建单元，更多操作见参考链接。

参考链接：<br>
http://www.360doc.com/content/19/0628/07/46368139_845317028.shtml

### 网络的训练参数提取
在网络调试的过程中，若需要对网络的训练参数进行提取或查看，可以使用以下方法
```
import torch.nn as nn
class TempNetwork(nn.Moudle):
...
temp_net = TempNetwork()
for name, param in temp_net.named_parameters():
    print(name, "    ", param.size())
```
这种方法可以同时查看网络中模块的名字，序号和权重名称以及权重的尺寸。这一方法只打印需要训练的权重。

参考链接：<br>
https://blog.csdn.net/Jee_King/article/details/87368398

### 随机数的生成
在测试网络的时候可以使用`torch.rand()`函数生成一个我们需要大小的张量，来输入网络，用于测试其参数是否正确，例如我们想模拟一个4x3x1024x2048的batch输入，可以使用`torch.rand(4, 3, 1024, 2048)`来生成。

参考连接：<br>
https://blog.csdn.net/dream161110/article/details/80293715