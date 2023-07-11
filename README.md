# xiaomi2meidi  小爱同学控制美的空调 


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
2023.7.11  最近发现点灯科技的接口频繁出现不稳定情况，本项目可能无法运行
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



1. 软件需常驻后台，建议运行在服务器上

2. 如果登录出现 1020 状态码，并且在手机重新登录无效 ，可以尝试把 config.json 中的 appVersion/deviceId/deviceName/osVersion 设置为空字符串

3. 配置修改后需重启程序

================================================================================================================


首先说一下思路.

想用小爱控制,那么最好的办法自然是接入小米的iot平台,然而小米并不对个人开发者开放.

退而求其次,我们可以找一个第三方厂商,由他们做中间人接入.

blinker (点灯科技) 是一家物联网技术提供商, 官网 点灯科技 (https://diandeng.tech/home)  (虽然文档烂,但是功能不含糊,该有的都有. )

虽然我之前参考 使用ESP32和Blinker实现远程网络唤醒电脑(接入语音助手，以小爱同学为例) 这篇帖子用esp32成功控制了电脑远程开机

但是这次我不想依赖硬件设备,不然成本太高了( 一台空调设备就得买一个esp32),

然后我就想blinker的代码既然能在树莓派上面跑,自然我就能把核心逻辑抠出来用java重写一遍

好.正文开始

================================================================================================================

首先我们在blinker官网下载 他们的app,注册.

注册后 登录,显示如下界面

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214412765-1101047799.jpg)
你们这里应该是空的,我加过所以有其他设备

然后我们点击右上角的 加号 进行添加一个新的设备

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214435190-316562765.jpg)

点击 独立设备

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214439546-953173405.jpg)

选择网络接入

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214444553-1124195692.jpg)

得到 authKey, 保存好,后面要用

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214448118-292088096.jpg)

然后我们返回设备列表,点击刚新加的设备. 右上角 三个点

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214451612-1672691443.jpg)

编辑设备名称

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214454329-294511756.jpg)

输入名称, 这个名称就是后面你喊小爱的名称,同时 要和 美的美居 app 里面空调的名称要相同

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214458390-1237804476.jpg)

确认修改后,我们下载 github 上的 程序 ,运行后在 程序提示的配置文件中输入

phone: 美的app手机号, password: 美的app密码,acNameList: 空调名称(多个用逗号隔开), blinkerKeyList: 点灯的authkey(多个用逗号隔开,需要与空调名称一一对应)
```
{
        "phone":"13812345678",
        "password":"123456",
        "acNameList":"书房空调,主卧空调,次卧空调",
        "blinkerKeyList":"8*****2，2*****9,0******8",
        "uid":"",
        "accessToken":"",
        "tokenPwd":"",
        "homeId":"",
        "appVersion":"",
        "deviceId":"",
        "deviceName":"",
        "osVersion":"",
        "deviceList":[]
}
```
![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214501893-836525093.jpg)

程序正常运行后.

我们 打开 米家 app , 点击 "我的" , 往下翻 选择 "其他平台设备"

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214504856-256340649.jpg)

先点添加, 找到 点灯科技. ,然后点击 同步设备.

![](https://img2022.cnblogs.com/blog/695883/202209/695883-20220912214507837-1815736572.jpg)
如果这里出现了你刚新加的设备说明就成功了.

然后就可以用小爱控制了

