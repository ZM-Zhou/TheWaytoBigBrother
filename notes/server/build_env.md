服务器的初始化和环境搭建
===
### 查看服务器基本信息
连接到服务器后，可以使用<br>
`lsb_release -a`命令查看服务器的Ubuntu版本<br>
`cat /usr/local/cuda/version.txt`命令查看服务器的CUDA版本<br>
`uname --m`命令可以查看系统是32位还是64位<br>
了解服务器的基本情况后方便下面进行环境的配置

### 虚拟环境的搭建
有时候不同的模型经常需要不同的框架，而不同的框架又对应了不同的python版本和支持库，所以一般会在服务器中配置虚拟环境，方便使用不同的版本，这里以anaconda为例<br>
首先在anaconda官网下载与服务器对应版本的安装文件，并发送到服务器<br>
之后在安装包目录下运行<br>
`bash ./Anaconda3-2019.10-Linux-x86_64.sh`<br>
一路yes回车即可完成安装<br>
在安装完之后，可以通过`conda info`来实验是否安装成功，这时候有概率出现找不到conda命令的情况，这是由于conda的路径还没有被添加到环境变量中，可以通过以下方式进行操作。<br>
`echo 'export PATH="/home/[user_name]/anaconda3/bin:$PATH"' >> ~/.bashrc`<br>
`source ~/.bashrc`<br>
将anaconda的路径添加到系统路径中即可。

### 虚拟环境的建立
在anaconda下，我们就可以建立所需的环境并进行配置了<br>
`conda create -n [env_name] python=[version]`新建一个虚拟环境<br>
之后在里面使用`conda install`或者`pip install`命令装所需的库即可

参考连接：<br>
https://www.jianshu.com/p/ad59b7d120ca<br>
https://blog.csdn.net/xianglao1935/article/details/80510494

