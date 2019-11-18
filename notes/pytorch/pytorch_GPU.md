通过pytorch查看可使用的GPU
===
在训练前，我们可以通过pytoch自带的CUDA相关方法来检查GPU的设置是否和我们预期的一致。  
```
import torch
torch.cuda.is_available()
torch.cuda.device_count()
```  
两个方法分别返回cuda是否可用（即是否找到了GPU），以及可用的GPU的数量。
需要注意的是，经过实验，发现如果在程序中使用`os.environ`设置CUDA的可见GPU，需要在`import torch`之前进行设置，否则将不起效。同理在`import torch`后也不能通过修改`os.environ`来改变CUDA的可见GPU。

参考链接：  
https://blog.csdn.net/nima1994/article/details/83001910

