github下载太慢的可以去百度网盘试试
https://pan.baidu.com/s/1dL2E4PsE97kRrrg7o6OI0Q 提取码: 8ruw

包含：
Cobalt_Strike4.5源码
Cobalt_Strike4_5_crack（直接食用）
二开源码



#### 启动命令汇总

##### 客户端启动

```bash
# 一般启动：
java -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -jar Cobalt_Strike4.5_crack.jar

# windows 带依赖启动：
java -Dfile.encoding=UTF-8 -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -cp ".;Cobalt_Strike4.5_crack.jar;lib/*" aggressor.Aggressor

# linux 带依赖启动：
java -Dfile.encoding=UTF-8 -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -cp ".:Cobalt_Strike4.5_crack.jar:lib/*" aggressor.Aggressor

# 使用UTF-8编码，中文版需要加这个
-Dfile.encoding=UTF-8

# 修复CVE-2022-39197（XSS-RCE）
-javaagent:patch.jar
```

##### 服务端启动

```bash
# 普通服务端启动
java -XX:ParallelGCThreads=4 -Dcobaltstrike.server_port=服务端端口 -Djavax.net.ssl.keyStore=./cobaltstrike.store -Djavax.net.ssl.keyStorePassword=SSL密码 -server -XX:+AggressiveHeap -XX:+UseParallelGC -classpath ./Cobalt_Strike4.5_crack.jar server.TeamServer 服务端IP 服务端密码

# windows 服务端带依赖启动：
java -XX:ParallelGCThreads=4 -Dcobaltstrike.server_port=服务端端口 -Djavax.net.ssl.keyStore=./cobaltstrike.store -Djavax.net.ssl.keyStorePassword=SSL密码 -server -XX:+AggressiveHeap -XX:+UseParallelGC -cp ".;Cobalt_Strike4.5_crack.jar;lib/*" server.TeamServer 服务端IP 服务端密码

# linux 服务端带依赖启动：
java -XX:ParallelGCThreads=4 -Dcobaltstrike.server_port=服务端端口 -Djavax.net.ssl.keyStore=./cobaltstrike.store -Djavax.net.ssl.keyStorePassword=SSL密码 -server -XX:+AggressiveHeap -XX:+UseParallelGC -cp ".:Cobalt_Strike4.5_crack.jar:lib/*" server.TeamServer 服务端IP 服务端密码
```

###### windows客户端启动脚本 cobaltstrike.vbs

```bash
Dim objShell
Set objShell = CreateObject("WScript.Shell")
objShell.Run "cmd.exe /K java -Dfile.encoding=UTF-8 -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -cp '.;Cobalt_Strike4.5_crack.jar;lib/*' aggressor.Aggressor", 0, False
Set objShell = Nothing
```

###### 服务端启动脚本 start.sh

```shell
nohup ./teamserver 服务端IP 服务端密码 > output.log 2>&1 &
```

###### 服务端停止脚本stop.sh，根据进程关闭，需要设置启动时的参数

```shell
#!/bin/bash

# 设置关键词（请自行修改）
KEYWORD1="./teamserver"
KEYWORD2="./TeamServerImage"
KEYWORD3="cobaltstrike.store"

# 查找匹配的进程PID
PIDS=$(ps -ef | grep -E "$KEYWORD1|$KEYWORD2|$KEYWORD3" | grep -v grep | awk '{print $2}')

# 判断是否找到匹配的进程
if [ -z "$PIDS" ]; then
    echo "未找到包含关键词 \"$KEYWORD1\" 或 \"$KEYWORD2\" 或\"$KEYWORD3\" 的进程"
else
    for pid in $PIDS
    do
        kill -9 "$pid" && echo "成功 kill 进程 PID: $pid" || echo "kill 进程 PID: $pid 失败"
    done
fi
```



#### 已修改说明

- 已修改默认心跳时间、随机百分比为20秒、50%，需要修改请前往：resources/default.profile


```bash
set sleeptime "20000";
set jitter    "50";
```

- 支持双语


- 修复中文版会话中输入help返回乱码问题


- VNC随机端口，自行前往***./resources/aggressor.prop***修改***client.vncports.string***字段
- 修复host_stage设置为false，使用提权时，无法选择监听器

- checksum8特征修改
- 谷歌认证登录
- 已修复CVE-2022-39197（XSS-RCE）
- 防DDos



#### 待修改（基本上都是些UI的改动，没啥必要，想改的自己去改吧）

- 定制profile，抓取目标网站流量，模仿其get、post定制profile，然后在客户端应用至服务端，监听器那里就设置为定制的profile，https://cobalt-strike.github.io/community_kit/中有该功能的插件，整合过来即可
- 上线IP归属地显示，感觉不是很必要
- UI风格
- 进程浏览，进程自动识别，尤其是杀软，用明亮颜色显示
- 文件浏览，特殊文件（文件名、后缀）用明亮颜色显示，举例，文件名：flag、user(name)、admin、administrator(s)、passwd、password、config等；后缀名：zip、rar、conf等，满足其一即可用红色显示
- 命令行多色显示
- 右键会话添加备注，弹出的备注改为多行文本，表格里面也是，并设置自动换行



#### 谷歌认证登录

- 可以通过谷歌认证APP扫码、或APP输入key
- 还可以把key写入客户端config/authcode_client.txt中，登录时勾选自动，会自动随authcode的变化填充，只需点击连接即可登录，方便快捷又安全。攻防时将服务端密码、key发给队友，即可快乐的多人运动了。



#### 已修复CVE-2022-39197（XSS-RCE）

参考4.9.1版本，修改源码修复了该漏洞，已通过修改源码中user为img标签测试，请求被丢弃，不放心的可以用下面方式：

来自：https://github.com/burpheart/CVE-2022-39197-patch

将 patch.jar 放入cobaltstrike启动目录下

在cobaltstrike启动参数中加入javaagent 启用补丁

```bash
-javaagent:patch.jar
```

启动cobaltstrike 输出Successfully Patched. 即为禁用成功

```bash
====== CVE-2022-39197 patch @burpheart ======
Successfully Patched.
```

这种是直接禁用了swing的html渲染，总感觉不太优雅



#### 防DDos

##### DDos思路

- 通过手动多次运行木马文件，或通过多线程/进程运行木马文件，来达到上线骚扰的目的（易实现）


- 通过发送上线流量，伪装上线（难实现）

##### 防DDos思路：

具体功能见下图

- **自动防护：**当请求间隔小于访问频率时触发，触发后会拒绝此次请求，并自动转为***仅已连接***

- **禁止所有连接：**拒绝所有请求；
- **控制访问频率：**当请求间隔小于访问频率时触发，触发后拒绝该请求；
- **仅已连接：**仅放行已连接的请求，其他一律拒绝；
- **访问频率：**即上一次访问与本次访问间隔时间，默认为***2秒/次***；
- **最大可连接数：**最大可记录最近连接的个数，默认为***100个***，***为0时拒绝所有请求***，这个是记录在内存当中，大小要适当；
- **应用设置：**向服务器同步***访问频率、最大可连接数***；
- **清除已连接：**清除已记录的连接，如果当前为***仅已连接***，清除后就会拒绝所有请求。

###### 使用建议

- 默认不启用任何模式，放行所有请求，攻防环境时建议开启自动防护

