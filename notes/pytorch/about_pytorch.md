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

### torch.nn.functional.interpolate()
对某个张量进行上下采样，一个常用的例子是对三维的tensor或四维的batch做尺寸的缩放，如下：<br>
`a = torch.nn.functional.interpolate(a, scale_factor=2, mode="bilinear", align_corners=True)`<br>
上面的用法对应将a放大两倍，使用双线性插值，使用角落对齐，是上采样比较常用的方式。<br>
函数也可以使用`size=`来传入目标尺寸进行缩放。需呀注意的是，进行下采样的时候使用`mode="nearest"`方式比较多。对于`align_corner`的详细解释见参考连接。

参考链接：<br>
https://blog.csdn.net/wangweiwells/article/details/101820932

### torch.nn.functional.one_hot()
将某个索引张量转换为one_hot形式的索引，可以用来从Id形式的label转换为mask形式的label，用法如下：<br>
```
mask_label = torch.nn.functional.one_hot(id_label, num_classes = class+1)
mask_label = mask_label.permute(0, 3, 1, 2)
```
需要注意的，如上面例子所示，其生成的one_hot会保留原始尺寸，并将索引放在最后一个维度，如果想变成常规tensor的格式，需要进行一下维度的交换。

