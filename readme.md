#helloworld: 快速入门，如何实现下行云控制


##概述

这个例子是演示如何能够 控制应用板上两个LED灯，通过本地 或者 远程都可以实现。

![wifiIO](../../addons_img/wifiIO_coredev_v2_back.png)


##使用说明

应用板上两盏LED灯实际就连接到IO1和IO2。

###编译、部署和运行

在云端登录自己的账户，确定设备在线后，可以使用编译器来编译上面的代码，部署时确定名称为“hello”（下面要用到），然后 运行之。

这时应该可以看到模块上的LED灯开始闪烁，闪烁若干次之后应该就会停止，这时我们可以尝试向模块发送命令来控制LED。



###本地控制

在“我的设备”界面找到模块本地的IP地址，访问模块主页。在“应用程序”->“委托接口测试” 填入：

    {
        "method":"hello.onoff",    //必须是Addon的运行名称
        "params":"on" //或者 "off"
    }

**注意，注释部分需要删除**

点击发送，便可以看到LED变化。这个演示了通过模块的http服务实现JSON RPC的调用。


###远程控制

登录wifi.io网站，“文档资源” -> “调试openAPI”  ，按照说明文字，首先获取Token，之后选择 “向设备发送指令”的API接口。
填入类如：

    {
        "token":"xxxx", //获取的token
        "did":xx, //你的设备编码
        "method":"hello.onoff",
        "params":"on" //或者"off"
    }

**注意，注释部分需要删除**

提交后便可以看到LED变化。

****
##关于JSON_RPC
代码中有定义

    int JSON_RPC(onoff)(jsmn_node_t* pjn, fp_json_delegate_ack ack, void* ctx)  

这实现了一个回调接口，该接口输入是请求的JSON串，输出可以是硬件动作亦或是JSON输出。

该接口定义了Addon中的一个method方法，可以被本地调用 也可以被云端访问。

* 本地控制的原理是：模块内部运行的http服务器接受了RPC请求，调用了Addon中的 JSON RPC 接口“onoff”。
* 远程控制的原理是：云端下发了JSON串，通过模块内部运行的ot客户端，调用了Addon中的 JSON RPC 接口“onoff”。

可见，使用wifiIO的运行框架，可以非常简单的实现代码复用，实现一次编写便可以在本地和远程被调用的实际效果。知道了这个基本原理我们就可以任意设计自己的下行应用程序了。



更多细节请参考源代码。

20131006
问题和建议请email: dy@wifi.io 