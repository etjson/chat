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
 _____   _   _       ___   _____
/  ___| | | | |     /   | |_   _|
| |     | |_| |    / /| |   | |
| |     |  _  |   / / | |   | |
| |___  | | | |  / /  | |   | |
\_____| |_| |_| /_/   |_|   |_|
--------------------------------------------------
chat.server
url: https://github.com/etjson/chat
version: 1.1
--------------------------------------------------
[root@centos chat]# [INFO] Process[redis.0] start.
[INFO] Process[jsencode.0] start.
[INFO] Worker#0 started.
[INFO] WebSocket Server listening at 127.0.0.1:35585
[INFO] HTTP Server listening at 0.0.0.0:9090
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
aiurl =
apilist =
rw_pc =
rw_mobile =
```

`core`节点：

`port`：聊天室开放端口

`reids`：缓存服务开放端口(给程序用的)

`key`：聊天内容加密秘钥，不填则自动随机生成

`adminpwd`：管理员密码，输入成为管理员指令需要用到

`aiurl`：机器人交互回调接口

`apilist`：自定义扩展

`rw_pc`：电脑端伪静态规则

留空默认如下：
```text
/i/[id].html
```

`rw_mobile`：手机端伪静态规则

留空默认如下：
```text
/ii/[id].html
```

## 自定义扩展

需配置`apilist`，并且对应配置。

以下为配置例子：

```text
apilist = 1,2

[api_1]
url = http://xxxxx.com/api.php
title = 测试1
admin =
input =
tip =

[api_2]
url = http://xxxxx.com/api.php
title = 测试2
admin = 1
input = 1
tip = 这里是输入框提示
```

`apilist`：填写的是id数组，以逗号分隔，填写1，那么就要对应有`[api_1]`的配置。

`url`：填写回调地址，例如用户点击的是测试1，那么接口接收的`POST`内容如下：

data为用户在输入框输入的内容，测试1没有开启输入框所以为空

```text
{
    "id": "1",
    "data": "",
    "fid": "123",
    "uid": "10001"
}
```

`admin`：不为空则必须管理员才可操作

`input`：不为空则是需要输入框输入内容

`tip`：不为空则是输入框的提示内容，换行用`&#10;`符号

接口输出需要`json`格式，且特定格式如下：

```text
{
    "type": "alert",
    "data": "提示内容"
}
```

`type`：alert为弹窗，msg为提示框，ai为机器人提示

`data`：为提示内容

## 其它配置

在根目录下创建`index.tpl`文件可以自定义首页内容。

在根目录下创建`welcome.txt`可自定义机器人欢迎语句。

## 聊天指令

需配置`aiurl`回调接口，指令发送的内容会通过`post`方式请求回调接口。

以下为用户发送的指令以及回调接口接收的内容：

```text
#ai 这里是内容
```

```text
{
    "data": "这里是内容"
    "fid": "123",
    "uid": "10001",
    "admin": "0"
}
```

接口输出直接为字符串即可，无需特定格式。

## 注意事项

所有配置文件需使用`UTF-8`编码编辑保存，否则异常。
