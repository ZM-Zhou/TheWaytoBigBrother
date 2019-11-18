在服务器上进行调试
===
### VS Code的初始化和配置
一般的网络训练会在服务器上进行，使用VS Code进行远程的运行和调试是很方便的。  
VS Code的SSH设置和初始化可以参考：  
https://www.jianshu.com/p/0f2fb935a9a1  
完成初始化和服务器端的python组件安装，即可使用VS Code进行在线调试，更多VS Code相关使用技巧可以参考：  
https://www.cnblogs.com/shine-lee/p/10234378.html  

### 在服务器上运行程序的三种方式
一般常用三种方式在服务器端调试程序：  
##### debug
在完成了VS Code的Python环境设置之后，可以在服务器端运行debug，方便使用断点进行调试。  
需要注意的是，和本地端相同，如果需要运行的时候传入参数，可以通过`Debug`中的`Add Configurations`实现，当创建一个config时，会自动生成如下信息：  
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

        {
            "name": "Python:当前文件",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
        }
    ]
}
```
这时，我们可以根据需要在里面添加环境变量设置，例如GPU的可见，或需要传入的参数，一个例子如下：  
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

        {
            "name": "Python: train",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "env":{"CUDA_VISIBLE_DEVICES": "1"},
            "args":[
                "--batch_size", "8",
                "--num_workers", "8",
                "--log_frequency", "200",
                "--save_frequency", "4",
                "--num_epochs", "40",
            ]
        }
    ]
}
```

##### 终端直接运行
这种方式最为直接，在终端中用`python 文件名.py`即可运行，若需要设置环境可以在`python`前加入环境变量：上面的例子在终端中运行写为：  
```
CUDA_VISIBLE_DEVICES=1 python 文件名.py --batch_size 8 --num_workers 8 --log_frequency 200 --save_frequency 4 --num_epochs 40
```
需要注意的是由于是远程运行，若连接被切断，该终端也会被关闭，程序自然会停止运行。由于连接并非那么可靠，且动辄十几个小时的训练不可能一直保持连接，所以最终真正训练的时候，需要使用最后一种方式进行调试。

##### 后台运行
在这种方式下，一般先将商议中方式的运行命令写入一个`sh`脚本文件，并给该文件授权运行:  
`chmod +x ./name.sh`  
之后在终端使用如下命令进行阻止挂起地后台启动:  
`nohup ./name.sh &`  
这样程序将自动运行直到结束，运行的输出结果会被保存到同目录下的`nohup.out`中。