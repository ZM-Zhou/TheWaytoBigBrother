LINUX基本操作
===
### 查询磁盘使用情况或文件情况
`df -h`<br>
其中参数-h表示查看每个根路径的分区大小，磁盘一般挂在sda中，向服务器上传大的数据集之前最好先查询一下。

`du -sh *`<br>
显示当前路径中所有文件夹的名字和大小。在上传文件的时候可以利用该指令查看文件的大小，以此判断是否传输完成。  

`ls -l |grep "^-"|wc -l`  
该命令可以查看当前文件夹下的文件数量，命令的解释如下：  
`ls -l`长列表输出该目录下文件信息(注意这里的文件，不同于一般的文件，可能是目录、链接、设备文件等)  
`grep "^-"`这里将长列表输出信息过滤一部分，只保留一般文件，如果只保留目录就是 ^d  
`wc -l`统计输出信息的行数，因为已经过滤得只剩一般文件了，所以统计结果就是一般文件信息的行数，又由于一行信息对应一个文件，所以也就是文件的个数。
参考链接：  
https://www.cnblogs.com/php-linux/p/10807407.html

### 软连接的使用
Linux系统中的软连接可以理解为windows系统中类似快捷方式的存在。其可以指定文件或文件夹指向某一特定路径，实现更简单更方便的调用。其建立方式如下：  
`ln -s [源地址] [目的地址]`  ，例如需要在MaskRCNN的程序里面建立一个指向cocoapi的软连接，可以使用如下指令：  
`ln -s /home/user_name/cocoapi/PythonAPI/pycocotools ./pycocotools`  
需要注意的是，源地址必须为绝对路径，否则可能会建立失败，并且软连接的建立是以目的地址新建一个文件（夹）这点需要注意。  
当然，还可以通过软连接和路径优先级，来实现使用不同版本编译器等功能，例如[gcc/g++个人版本修改]](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/multiple_gcc_g++.md)就是一个例子。  

### 文件的复制
`cp [源地址] [目的地址]`  
该命令可以进行文件的复制

参考链接：  
https://www.cnblogs.com/aiyr/p/7395738.html

### 输出的重定向
在进行网络训练，尤其是在同一个路径下使用`nohup &`进行后台运行的时候，会出现多个进程的输出结果都被保存在同一个'nohup.out'中的情况。为了防止这样情况的产生，可以使用linux的输出重定向功能将其重新指定输出路径，一个常用的实现如下：<br>
```
filename="./nohup_log/`date +%y_%m_%d_%H_%M_%S`.txt"
python xxx.py\
 2>&1 | tee $filename
```
具体解释来说，首先对存储的文件进行规定，在当前文件夹下建立一个`nohup_log`文件，在其中建立以时间命名的文件。之后，通过`>`符号，将文件产生的输出送入刚才建立的文件中。更多的细节可以参考下面链接。

参考链接：<br>
https://blog.csdn.net/liucy007/article/details/90207830

### Nvidia GPU情况查询
`nvidia-smi`<br>
通过该指令可以查看NVIDIA显卡当前的工作状态，具体参数解释见下面的参考链接。<br>
如果需要动态实时显示，可以采用如下方式：<br>
`watch -n 0.2 nvidia-smi`<br>
其中0.2是刷新的间隔，单位为秒。<br>
如果不幸服务器的某一块GPU挂掉了，那么该GPU之后的GPU就无法通过上述方式查看，但可以通过以下方式来查看某个具体的没有挂掉的GPU。<br>
`nvidia-smi --id=n`

参考连接<br>
https://blog.csdn.net/u013498580/article/details/80090103 <-关于各项参数的解释
https://zhuanlan.zhihu.com/p/53345706