服务器的初始化和环境搭建
===
### 查看服务器基本信息
连接到服务器后，可以使用<br>
`lsb_release -a`命令查看服务器的Ubuntu版本<br>
`cat /usr/local/cuda/version.txt`命令查看服务器的CUDA版本<br>
`cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`命令查看服务器CUDNN版本<br>
`uname --m`命令可以查看系统是32位还是64位<br>
了解服务器的基本情况后方便下面进行环境的配置

### 网络连接与网卡驱动
如果不幸使用的服务器/电脑联网有问题，请首先检查网线和网口是否都是好的，之后查看电脑的网卡配置是否有问题<br>
以Ubuntu 16.04为例，电脑最开始无法联网，首先可以用`lspci | grep network`查看目前主机上的网络硬件设备<br>
检查网卡驱动若与硬件不符，可以尝试更新网卡的驱动，具体过程见参考连接。

参考链接：<br>
https://blog.csdn.net/bocai0529/article/details/102894075->驱动的下载和更换

### CUDA的安装
尝试使用命令`nvidia-smi`查看当前服务器NVIDIA GPU的情况，发现该指令不存在，通过Ubuntu系统的系统设置->更新和->附加驱动发现并没有安装NVIDIA CUDA的相关驱动，那么就需要进行重新安装。<br>
在检查完电脑是否具备安装条件之后，可以按照参考连接的方法进行安装。安装的过程中注意不要安装nvidia accelerated graphic driver 和 openGL！！！<br>

参考链接:<br>
https://blog.csdn.net/qq_39521554/article/details/82829886<br>
https://blog.csdn.net/weixin_42279044/article/details/83181686 ->更详细的安装过程

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
这时候，有可能出现报错`Permission...`，提示当前用户的权限不够。虽然原因不明，但我们可以通过使用管理员权限为当前用户分配anaconda的使用权限来解决这个问题。<br>
在上述报错中，会提示当前用户的UID和PID，之后，可以使用如下命令给该用户分配anaconda的使用权限<br>
``<br>
之后在里面使用`conda install`或者`pip install`命令装所需的库即可

### 镜像的添加
有时候某些组件直接连接原网页下载，速度会很慢，这时候可以使用如下命令将清华镜像添加进来，通过镜像进行下载
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

参考连接：<br>
https://www.jianshu.com/p/ad59b7d120ca<br>
https://blog.csdn.net/xianglao1935/article/details/80510494<br>
https://blog.csdn.net/watermelon1123/article/details/88122020

###　疑难杂症
##### Ubuntu循环登录
电脑一直显示不出来核心显卡，在网上查找解决方案，貌似升级一下软件有可能解决问题。遂
```
sudo apt-get update
sudo apt-get upgrade
```
之后重启，遇到了循环登陆的问题，即不停重复回到登陆界面，而不进入桌面，继续查找解决方案，根据参考连接1，进入命令行界面，登陆后查找相应的报错文件。根据报错文件提示进行修复。<br>
我这里的报错显示`error of failed request badvalue`根据参考连接2，尝试卸载所有nvidia驱动，并重装xorg相关组件，但并未关闭图形界面并重装nvidia驱动就直接reboot<br>
导致Ubuntu无法启动，停留在如下界面：<br>
`/dev/sda1:clean XXXXXX/XXXXXX files, XXXXX/XXXXX blocks`<br>
根据网上的解决方法，可以通过命令行模式对软件进行升级或显卡驱动重装来解决问题，但该状态下无法进入命令行模式，不能进行任何操作。<br>
还有认为是挂载出现问题，需要修改/etc/fstab中的挂载信息，为了完成这个操作，可以根据参考连接3的步骤制作一个live系统，通过开机时F2选择U盘引导启动，即可修改相关信息，但打开fstab发现其中的内容正常。<br>
再次检索，偶然发现可以通过grub(开机的时候玄学按shift有机会进入)进入高级模式来通过root进入系统，具体方法见参考连接4，但在命令行再次升级软件以及重装xorg，并替换xorg.conf后（参考连接5），仍然没有作用。<br>
在上述所有操作后放置一段时间（真的就是静置）开机后还是停在上述的界面，但可以进入命令行模式！<br>
按照参考链接6的步骤重新下载NVIDIA驱动并安装（驱动可能会产生一个假报错（NVIDIA程序猿是真的狗），继续即可），重装CUDA，注意如果让安装openGL选No，之后重启即可进入图形界面。<br>
注：安装CUDA可能出现缺少动态链接库报错，按照参考链接7进行安装即可，最后有一个***Warning提示你所需NVIDIA驱动的最新版本，并不是报错，并且按他的提示其实CUDA的安装包可以安装相应版本的NIVIDIA驱动，不需要单独下载。

参考连接：<br>
https://blog.csdn.net/mangobar/article/details/82218807 No1<br>
https://www.kutu66.com/c/mutia_159615 No2<br>
https://askubuntu.com/questions/893922 No2<br>
https://www.wandouip.com/t5i164068/ No2->未尝试的修复方法，先标记一下<br>
https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows?_ga=2.147819767.170615197.1577497952-826573085.1577497952#9 No3<br>
https://blog.csdn.net/u013810296/article/details/86683559 No4<br>
https://www.it610.com/article/2193203.htm No5<br>
https://blog.csdn.net/u014561933/article/details/79958130 No6->驱动虚假报错<br>
https://blog.csdn.net/qxqsunshine/article/details/96907627 No6->驱动安装<br>
https://blog.csdn.net/zhang970187013/article/details/81012845 No6->驱动安装（无openGL）<br>
https://blog.csdn.net/10km/article/details/61915535 No7
