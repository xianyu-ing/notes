
## ���� sshd ����

SSH��Secure Shell����һ���ܹ��԰�ȫ�ķ�ʽ�ṩԶ�̵�¼��Э�飬Ҳ��ĿǰԶ�̹���Linuxϵͳ����ѡ��ʽ���ڴ�֮ǰ��һ��ʹ��FTP��Telnet������Զ�̵�¼��������Ϊ���������ĵ���ʽ�������д����˻������������Ϣ����˺ܲ���ȫ���������ܵ��ڿͷ�����м��˹�����������۸Ĵ����������Ϣ������ֱ��ץȡ���������˻����롣

���ְ�ȫ��֤�ķ�����

* ���ڿ������֤�����˻�����������֤��¼��
* ������Կ����֤����Ҫ�ڱ���������Կ�ԣ�Ȼ�����Կ���еĹ�Կ�ϴ���������������������еĹ�Կ���бȽϣ��÷�ʽ�����˵����ȫ��

sshd �����������Ϣ������ `/etc/ssh/sshd_config` �ļ���

    sshd���������ļ��а����Ĳ����Լ�����
    
    

����                              | ����
----------------------------------|-------------------
Port 22                           | Ĭ�ϵ�sshd����˿�
ListenAddress 0.0.0.0             |	�趨sshd������������IP��ַ
Protocol 2                        |	SSHЭ��İ汾��
HostKey /tc/ssh/ssh_host_key      |	SSHЭ��汾Ϊ1ʱ��DES˽Կ��ŵ�λ��
HostKey /etc/ssh/ssh_host_rsa_key |	SSHЭ��汾Ϊ2ʱ��RSA˽Կ��ŵ�λ��
HostKey /etc/ssh/ssh_host_dsa_key |	SSHЭ��汾Ϊ2ʱ��DSA˽Կ��ŵ�λ��
PermitRootLogin yes               |	�趨�Ƿ�����root����Աֱ�ӵ�¼
StrictModes yes                   |	��Զ���û���˽Կ�ı�ʱֱ�Ӿܾ�����
MaxAuthTries 6                    |	������볢�Դ���
MaxSessions 10                    |	����ն���
PasswordAuthentication yes        |	�Ƿ�����������֤
PermitEmptyPasswords no           |	�趨sshd������������IP��ַ

������ֹ�� root �˺ŵ�¼������������ PermitRootLogin yes ȥ�� '#' �� �Ѳ��� yes �ĳ� no ��ο�һ�²�����

```
[root@10-46-99-234 ~]# vim /etc/ssh/sshd_config
 46 
 47 #LoginGraceTime 2m
 48 PermitRootLogin no
 49 #StrictModes yes
 50 #MaxAuthTries 6
 51 #MaxSessions 10
 52
 ������������ʡ�Բ��������Ϣ������������
```
�����˳������� ssh ����

```
[root@10-46-99-234 ~]# systemctl restart sshd
[root@10-46-99-234 ~]# systemctl enable sshd
```

## ��ȫ��Կ��֤

����ԭ��
    
    1.AҪ��B������Ϣ��A��B��Ҫ����һ�����ڼ���
    2.A��˽Կ���ܣ�A�Ĺ�Կ����B��B��˽Կ���ܣ�B�Ĺ�Կ����A��
    3.AҪ��B������Ϣʱ��A��B�Ĺ�Կ������Ϣ����ΪA֪��B�Ĺ�Կ��
    4.A�������Ϣ����B���Ѿ���B�Ĺ�Կ������Ϣ����
    5.B�յ������Ϣ��B���Լ���˽Կ����A����Ϣ�����������յ�������ĵ��˶��޷����ܣ���Ϊֻ��B����B��˽Կ��
    
���ò���

1. �ڿͻ������������� ����Կ�ԡ�
    
```
[root@linuxprobe ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):���س�����������Կ�Ĵ洢·��
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):ֱ�Ӱ��س�����������Կ������
Enter same passphrase again:�ٴΰ��س�����������Կ������
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
