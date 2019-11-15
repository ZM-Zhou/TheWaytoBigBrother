LINUX基本操作
===
### 查询磁盘使用情况
`df -h`<br>
其中参数-h表示查看每个根路径的分区大小，磁盘一般挂在sda中，向服务器上传大的数据集之前最好先查询一下。

`du -sh *`<br>
显示当前路径中所有文件夹的名字和大小。在上传文件的时候可以利用该指令查看文件的大小，以此判断是否传输完成。

### 软连接的使用
Linux系统中的软连接可以理解为windows系统中类似快捷方式的存在。其可以指定文件或文件夹指向某一特定路径，实现更简单更方便的调用。其建立方式如下：  
`ln -s [源地址] [目的地址]`  ，例如需要在MaskRCNN的程序里面建立一个指向cocoapi的软连接，可以使用如下指令：  
`ln -s /home/user_name/cocoapi/PythonAPI/pycocotools ./pycocotools`  
需要注意的是，源地址必须为绝对路径，否则可能会建立失败，并且软连接的建立是以目的地址新建一个文件（夹）这点需要注意。  
当然，还可以通过软连接和路径优先级，来实现使用不同版本编译器等功能，例如[gcc/g++个人版本修改]](https://github.com/ZM-Zhou/TheWaytoBigBrother/tree/master/notes/server/multiple_gcc_g++.md)就是一个例子。  