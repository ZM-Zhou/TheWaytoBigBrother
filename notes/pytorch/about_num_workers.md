num_workers相关
===
一般Dataloader中会需要传入num_workers这一参数，决定Dataloader使用多少个线程同时进行数据的处理。linux的多线程自带队列管理，可以使用多个workers，一般的经验是使用4个或8个，取决于服务器的cpu能力。<br>
需要注意的是，在windows系统下进行调试的时候，一般设置num_workers为0，因为windows并不具有队列管理，在没有特殊维护的情况下使用多个worekers会导致报错。

参考连接：  
https://discuss.pytorch.org/t/guidelines-for-assigning-num-workers-to-dataloader/813