通向大佬的路-踩坑&经验
===
作为一个非CS专业，入坑CV一年多，从一个啥啥都不会的小白，变成了大概知道自己在干嘛的小菜鸡。一年里面踩了不少坑，也见了一些稀奇古怪的报错和问题。想在这里做一个记录和简单梳理，希望能帮到有需要的人。<br>
不得不说我并对整理并不太擅长，也没有能力像大佬们那样把所有东西写的那么透彻。基本是以能解决问题为目的，具体的内容都在相应的参考连接中。<br>
不定期更新，也欢迎各路大佬进行批评和指导！<br>
（尽量保持每周一更）

服务器的使用
---
[0.LINUX基本操作](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/linux_commands.md)<br>
查询磁盘使用情况或文件情况<br>
软连接的使用<br>
文件的复制<br>
输出的重定向<br>
Nvidia GPU情况查询<br>
CPU情况查询<br>
zip文件的压缩与解压

[1.服务器的初始化和环境搭建](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/build_env.md)<br>
查看服务器基本信息<br>
网络连接与网卡驱动<br>
CUDA的安装<br>
虚拟环境的搭建<br>
虚拟环境的建立<br>
镜像的添加<br>
疑难杂症

[2.在服务器上进行调试](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/debug_run_online.md)<br>
VS Code的初始化和配置<br>
在服务器上运行程序的三种方式

[3.同一个服务器中使用多个gcc, g++版本](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/multiple_gcc_g++.md)

[4.利用Github进行代码版本管理](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/github.md)<br>
将在线代码库拉取到本地<br>
分支操作<br>
提交和拉取<br>
版本回溯<br>
合并提交

pytorch学习记录
---
[0.一些常用的python操作](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/about_python.md)<br>
关于维度交换和改变<br>
关于数据类型转换<br>
关于张量复制与变形<br>
关于numpy的反转操作<br>
关于numpy的逻辑运算<br>
关于numpy的矩阵操作<br>
字典（dict）与元组（tuple）<br>
列表的查找<br>
列表的排序<br>
pyhon的面向对象<br>
python字符串引用<br>
python正则化匹配

[1.关于python编程建议的备忘](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/python_suggestions.md)<br>
== 与 is 的区别<br>
擅用 with 来关闭资源<br>
使用 else 判断循环退出<br>
关于None<br>
大规模的字符串连接优先使用join方法<br>
字符串格式化方法 .format()<br>
可变对象/不可变对象与参数传递<br>
列表，字典和元组的初始化<br>
区分浅拷贝和深拷贝

[1.python的图像操作扩展](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/python_and_images.md)<br>
关于图片加载和显示<br>
图片格式的变换<br>
图片的变换<br>
关于训练结果的对比输出<br>
关于色彩映射

[2.Pytorch功能小计](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/about_pytorch.md)<br>
torch.stack()<br>
torch.where()<br>
torch.argmax()<br>
torch.nn.functional.interpolate()<br>
torch.nn.functional.one_hot()

[3.pytorch通过GPU进行训练](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/pytorch_GPU.md)<br>
GPU基本情况的查询<br>
pytorch查看GPU情况<br>
更高效的GPU训练<br>
ReLU中的inplace

[4.num_workers相关](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/about_num_workers.md)<br>

[5.在Pytorch中使用tensorboard](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/pytorch_with_tensorboard.md)<br>

[6.Pytorch关于learning rate和optimizer](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/more_about_learning_rate.md)<br>

[7.Pytroch的debug技巧](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/pytorch/pytorch_debug.md)<br>
VS Code使用jupyter notebook<br>
网络的训练参数提取<br>
随机数的生成