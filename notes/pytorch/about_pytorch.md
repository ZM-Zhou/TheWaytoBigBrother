Pytorch功能小计
===
### torch.stack()
torch中的stack，可以将张量按某个特定维度进行拼接，使用格式如下:<br>
`stack_tensor = torch.stack((t1, t2, t3), dim=1)`<br>
需要注意合并对其他维度有尺寸的限制，需要相同。

参考连接：<br>
https://blog.csdn.net/Teeyohuang/article/details/80362756

### torch.where()
用法与`np.where()`完全相同，需要注意的是其所有的参数应该都是tensor类型。例如需要生成一个tensor的mask，可以采用如下方法：<br>
`mask_true = torch.where(torch.isnan(y_true), torch.full_like(y_true, 0), torch.full_like(y_true, 1))`

参考连接：<br>
https://blog.csdn.net/xijuezhu8128/article/details/86590562

### torch.argmax()
在某一个维度上对张量该维度上的值进行比较，并返回相应的索引，在返回的张量中该维度将会被压缩。使用范例：<br>
`b=torch.argmax(a,dim=0)`

参考连接：<br>
https://blog.csdn.net/weixin_42494287/article/details/92797061