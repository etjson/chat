# 聊天室

基于`hyperf`框架开发的一款web聊天室，双向数据加密，任何聊天信息不会存于客户端/服务端，开发这款产品的初衷就是全程`无痕`。

## 使用说明

下载压缩吧解压即可运行命令使用。

需要以`root`身份权限运行。

启动：

```shell
./run start
```

停止：

```shell
./run stop
```

重启：

```shell
./run restart
```

查询状态：

```shell
./run status
```

查询环境版本：

```shell
./run version
```

启动后的提示如下：

```text
[root@centos phptest]# ./run start
#!/usr/bin/env php
╔══════════════════════════════════════════════════╗
  chat.server
  url: https://github.com/etjson/chat/releases
  version: 1.0
╚══════════════════════════════════════════════════╝
[root@centos phptest]# [INFO] Worker#0 started.
[INFO] WebSocket Server listening at 0.0.0.0:33451
[INFO] HTTP Server listening at 0.0.0.0:9090
[INFO] Process[redis.0] start.
[INFO] Process[jsencode.0] start.
```

访问地址为：

电脑端：http://你的IP地址:配置的端口号/i/房间号.html

手机端：http://你的IP地址:配置的端口号/ii/房间号.html

## 配置文件

程序根目录下有`core.ini`文件，根据自己需求进行配置。

配置文件默认内容如下：

```text
[core]
port = 9090
redis = 6389
key =
adminpwd =

[bt]
url =
key =

[sms]
admin =
appid =
appkey =
template_admin =
template_sms =
sign =
```

`core`节点：

`port`：聊天室开放端口

`reids`：缓存服务开放端口(给程序用的)

`key`：聊天内容加密秘钥，不填则自动随机生成

`adminpwd`：管理员密码，输入成为管理员指令需要用到

`bt`节点：

PS：此配置为宝塔API接口配置，用于在聊天室中输入指令调用宝塔服务，配置时IP白名单别忘填写127.0.0.1(特殊情况除外)

`url`：宝塔API地址

`key`：宝塔API的秘钥

`sms`节点：

PS：此配置为腾讯云短信接口配置，用于在聊天室中输入指令调用短信发送服务

`admin`：管理员手机号

`appid`：应用appid

`appkey`：应用key秘钥

`template_admin`：短信通知管理员模板ID

管理员通知模板只会传递房间ID一个参数，所以推荐申请这个模板：

```text
亲爱的管理员，房间：{1}号呼叫，请及时查看！
```

`template_sms`：短信通知用户模板ID

短信通知用户模板没有参数要求，但是用户提供的参数值必须和模板中的对应，比如申请的模板是：

```text
亲爱的用户：{1}，您好，房间：{2}号呼叫，请及时查看！
```

这个模板中就包含了两个参数，此时管理员指令`#dingto`应该发送如下格式：

```text
#dingto 13800000000 用户A 房间B
```

`sign`：短信签名，不填则使用默认签名

## 聊天指令

`#setadmin 密码`：成为管理员

`#deladmin 密码`：取消管理员

`#delmsgall`：清空当前房间所有成员聊天记录(需管理员权限)

`#dingto 手机号码 内容`：发送手机端短信给指定手机号(需管理员权限)(需配置短信接口)

`#stopip IP地址`：封禁IP地址，用户无法进入任何房间(需管理员权限)

`#stopipre IP地址`：解封IP地址(需管理员权限)

`#getdata`：查看所有房间基本信息(需管理员权限)

`#getuid 用户UID`：查看某个用户的基本信息(需管理员权限)

`#wall`：查看防火墙信息(需管理员权限)(需配置宝塔接口)

`#ruin 端口号`：操作端口封禁(需管理员权限)(需配置宝塔接口)

`#ruinre 端口号`：操作端口解封开放(需管理员权限)(需配置宝塔接口)

`#private 用户UID 内容`：给别人发送私聊，不支持回复

`#my`：查看自己用户信息

`#ding`：通知管理员来当前房间(需配置短信接口)

`#ai 内容`：和AI机器人进行交互(需要连接外网)
