# mac环境下利用expect实现的自动登录脚本
## 场景
mac下的terminal终端简单而便捷，但是不能像windows下的xshell或crt那样，能够记录ssh连接的密码，导致每次登录都需要最原始的方式先 ssh user@host，然后终端提示输入密码，最后才能登录，非常麻烦。例如：   
```
$ ssh root@192.168.1.1
root@192.168.1.1's password: 
Last login: Tue Dec 17 23:12:40 2019 from 192.168.1.6

Welcome to aliyun Elastic Compute Service!
```

## expect
google后发现，大家都在使用shell下的工具expect来实现自动登录的功能。
> **expect** 是自动应答命令，用于交互式命令的自动执行。

简单地说，就是原先由系统决定的，必须先中断进程，由用户输入后再继续执行，必须人工干预，通过expect命令后，可以完全由shell脚本自动完成。
### 1.安装
```
brew install expect
```
验证
```
$ expect
expect1.1> 
```
### 2.使用
```
#! /usr/bin/expect
spawn ssh root@192.168.1.1
expect "password"
send "123456\n"
expect "*#"
interact
```
***解释***  
1. 第一行非常重要，如果没有将报错：`-bash: spawn: command not found`
2. 第二行，表示开始监控程序发生的交互
3. 第三行，当出现password时
4. 第四行，发送（模拟用户输入）密码，注意这里一定要加一个\n，否则程序运行时还需要人工手工回车一下，而且反应非常慢
5. 第五行，出现#时，表示登录上了
6. 第六行，将控制权交还给用户