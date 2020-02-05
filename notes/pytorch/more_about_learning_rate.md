Pytorch关于learning rate和optimizer
===
有时候出于训练需要，我们需要对不同的网络参数使用不同的学习率，这时候我们可以改变传入optim函数的参数，来实现这一功能，例如：  
```
optim.SGD([{'params': model.base.parameters()},
           {'params': model.classifier.parameters(), 'lr': 1e-3}], 
          lr=1e-2, momentum=0.9)
```
可以看到优化器的第一个参数可以传入一个字典list（注意，就算只有一个字典，也需要将其包在一个list中！！！），字典中的'params'对应应用该学习率的参数，'lr'对应学习率。若不设置'lr'则使用优化器lr参数传入的学习率。<br>
为了获取网络的部分变量，组成需要的list，我们可以配合正则表达式来进行截取，例如：  
```
for param in self.named_parameters():
            layer_name = param[0]
            not_trainable = bool(re.fullmatch("encoder.layer[1-4].1.*", layer_name))
            if not_trainable:
                param[1].requires_grad = False
```
这是一段选择部分层进行训练屏蔽的代码，将设置requires_grad改为加入一个paras_list即可截取部分训练变量。

还可以应用于统计模型的可训练参数：  
```
def get_parameter_number(net):
    total_num = sum(p.numel() for p in net.parameters())
    trainable_num = sum(p.numel() for p in net.parameters() if p.requires_grad)
    return {'Total': total_num, 'Trainable': trainable_num}

```
下面是一个训练前对优化器的设置过程。
```
self.model_optimizer = optim.Adam(self.network.get_trainable_params(), self.opts.learning_rate)
self.model_lr_scheduler = optim.lr_scheduler.StepLR(
                            self.model_optimizer, self.opts.scheduler_step_size, 0.1)
```
其中lr_scheduler允许在训练过程中调整学习率，需要注意的是，当使用`StepLR()`时，学习率的调整是每`step_size`调整一次，如果希望设置为在第`step_size`个epoch后调整，则只需将其`step()`方法放在epoch训练完之后即可。

参考链接：<br>
https://blog.csdn.net/weixin_42926076/article/details/99678948
