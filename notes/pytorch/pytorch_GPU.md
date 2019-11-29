pytorch通过GPU进行训练
===
### GPU基本情况的查询
[Linux基本操作](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/linux_commands.md)

### pytorch查看GPU情况
在训练前，我们可以通过pytoch自带的CUDA相关方法来检查GPU的设置是否和我们预期的一致。  
```
import torch
torch.cuda.is_available()
torch.cuda.device_count()
```  
两个方法分别返回cuda是否可用（即是否找到了GPU），以及可用的GPU的数量。<br>
需要注意的是，经过实验，发现如果在程序中使用`os.environ`设置CUDA的可见GPU，需要在`import torch`之前进行设置，否则将不起效。同理在`import torch`后也不能通过修改`os.environ`来改变CUDA的可见GPU。

参考链接：<br>
https://blog.csdn.net/nima1994/article/details/83001910

### 更高效的GPU训练
在训练中我们会发现有时候会出现GPU的占用率很低，现阶段的主要问题是载入数据的速度太慢，还未找到更好的提速方法。

参考连接：<br>
https://zhuanlan.zhihu.com/p/39752167 <-Dataloader num_workers和pin_mem的选择<br>
https://zhuanlan.zhihu.com/p/66145913 <-使用预加载提速<br>
https://zhuanlan.zhihu.com/p/53345706


