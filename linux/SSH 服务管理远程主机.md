
## 配置 sshd 服务

SSH（Secure Shell）是一种能够以安全的方式提供远程登录的协议，也是目前远程管理Linux系统的首选方式。在此之前，一般使用FTP或Telnet来进行远程登录。但是因为它们以明文的形式在网络中传输账户密码和数据信息，因此很不安全，很容易受到黑客发起的中间人攻击，这轻则篡改传输的数据信息，重则直接抓取服务器的账户密码。

两种安全验证的方法：

* 基于口令的验证—用账户和密码来验证登录；
* 基于密钥的验证—需要在本地生成密钥对，然后把密钥对中的公钥上传至服务器，并与服务器中的公钥进行比较；该方式相较来说更安全。

sshd 服务的配置信息保存在 `/etc/ssh/sshd_config` 文件中

    sshd服务配置文件中包含的参数以及作用
    
    

参数                              | 作用
----------------------------------|-------------------
Port 22                           | 默认的sshd服务端口
ListenAddress 0.0.0.0             |	设定sshd服务器监听的IP地址
Protocol 2                        |	SSH协议的版本号
HostKey /tc/ssh/ssh_host_key      |	SSH协议版本为1时，DES私钥存放的位置
HostKey /etc/ssh/ssh_host_rsa_key |	SSH协议版本为2时，RSA私钥存放的位置
HostKey /etc/ssh/ssh_host_dsa_key |	SSH协议版本为2时，DSA私钥存放的位置
PermitRootLogin yes               |	设定是否允许root管理员直接登录
StrictModes yes                   |	当远程用户的私钥改变时直接拒绝连接
MaxAuthTries 6                    |	最大密码尝试次数
MaxSessions 10                    |	最大终端数
PasswordAuthentication yes        |	是否允许密码验证
PermitEmptyPasswords no           |	设定sshd服务器监听的IP地址

例：禁止以 root 账号登录到服务器，将 PermitRootLogin yes 去掉 '#' 号 把参数 yes 改成 no 请参考一下操作：

```
[root@10-46-99-234 ~]# vim /etc/ssh/sshd_config
 46 
 47 #LoginGraceTime 2m
 48 PermitRootLogin no
 49 #StrictModes yes
 50 #MaxAuthTries 6
 51 #MaxSessions 10
 52
 ………………省略部分输出信息………………
```
保存退出，重启 ssh 服务

```
[root@10-46-99-234 ~]# systemctl restart sshd
[root@10-46-99-234 ~]# systemctl enable sshd
```

## 安全密钥验证

工作原理
    
    1.A要向B发送信息，A和B都要产生一对用于加密
    2.A的私钥保密，A的公钥告诉B；B的私钥保密，B的公钥告诉A。
    3.A要给B发送信息时，A用B的公钥加密信息，因为A知道B的公钥。
    4.A将这个消息发给B（已经用B的公钥加密消息）。
    5.B收到这个消息后，B用自己的私钥解密A的消息。其他所有收到这个报文的人都无法解密，因为只有B才有B的私钥。
    
配置步骤

1. 在客户端主机中生成 “密钥对”
    
```
[root@linuxprobe ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):按回车键或设置密钥的存储路径
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):直接按回车键或设置密钥的密码
Enter same passphrase again:再次按回车键或设置密钥的密码
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
40:32:48:18:e4:ac:c0:c3:c1:ba:7c:6c:3a:a8:b5:22 root@linuxprobe.com
The key's randomart image is:
+--[ RSA 2048]----+
|+*..o .          |
|*.o  +           |
|o*    .          |
|+ .    .         |
|o..     S        |
|.. +             |
|. =              |
|E+ .             |
|+.o              |
+-----------------+
```
