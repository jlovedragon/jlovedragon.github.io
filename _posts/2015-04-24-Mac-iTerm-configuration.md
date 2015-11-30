---
published: true
layout: post
title: Mac下配置iTerm2实现快速远程登录
categories: [Tool]
---

Mac上的神器太多，今天主要讲下如何配置iTerm2终端实现快速远程登录。

### **编写脚本**
需要用到[expect语法](http://linux.die.net/man/1/expect)，代码如下：

```python
#!/usr/bin/expect -f
set username YourUserName    // 设置用户名
set host ***.***.***.***     // 设置主机IP
set password YourPassword    // 设置密码
set time -1                  // 设置永不超时

spawn ssh $username@$host    // 启动一个新进程
expect "*assword:*"          // 进程返回带有assword:字符时
send "$password\r"           // 向进程输入前面设置的密码
interact                     // 允许用户交互
expect eof                   // 结束
```

保存到一个自己知道的目录，后面配置快捷键需要用到，我存放的目录是`~/program/myscripts/235.sh`。

### **配置iTerm2**
在iTerm2下按`command` + `o`（字母o，不是数字0），出现如下图：
![](http://ww4.sinaimg.cn/large/6120fe13jw1erjdj6y6z7j214u0l8n06.jpg)
点击框住的按钮`Edit Profiles...`，会出现下面的图，点击左下角的加号新建一个Profile，设置快捷键执行刚写好的脚本：
![](http://ww2.sinaimg.cn/large/6120fe13jw1erjdoa21p4j21fc0u0wnj.jpg)

### **测试**
我设置的是`ctrl` + `command` + `T`，按了组合键之后出现如下图：
![](http://ww4.sinaimg.cn/large/6120fe13jw1erjdx08q74j20vu0lyai4.jpg)
ssh成功登录，直接按快捷键就能快速登录~ 

### **配置文件下载**
[github链接](https://github.com/jlovedragon/profile/blob/master/mac/sshlogin.sh)
