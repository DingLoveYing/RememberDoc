1. 首先在服务器上安装ssh的服务器端。
$ sudo aptitude install openssh-server

2. 启动ssh-server。
$ /etc/init.d/ssh restart

3. 确认ssh-server已经正常工作。
$ netstat -tlp
tcp6 0 0 *:ssh *:* LISTEN -
看到上面这一行输出说明ssh-server已经在运行了。

4. 在Ubuntu客户端通过ssh登录服务器。假设服务器的IP地址是192.168.0.103，登录的用户名是hyx。
$ ssh -l hyx 192.168.0.103
接下来会提示输入密码，然后就能成功登录到服务器上了。


上传文件：
用ssh自带的scp或者sftp.
scp 本地文件 user@host:远程路径
