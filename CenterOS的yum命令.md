# CentOS 使用篇（一） 常见使用命令

## zsh

* [CentOs安装oh my zsh](https://www.jianshu.com/p/556ff130fc65)
* [centos 安装zsh 及其插件](https://iluoy.com/articles/133)

## yum

### 包的更新

1. 列出所有可更新的软件清单 `yum check-update`
2. 安装所有更新软件 `yum update`
3. 版本升级 `yum upgrade`
4. 更新 yum 缓存 `yum makecache`
5. 清除 yum 缓存 `yum cache`、`yum cache all`

### 包安装与删除

1. 安装 `yum install [package_name]`
2. 删除 `yum remove [package_name]`

### 包搜索

1. 搜索特定包 `yum search [package_name]`
2. 搜索包含特定文件名的包 `yum provides [name]`

### 包列表

1. 列出所有安装或更新的包 `yum list`
2. 列出指定包 `yum list [name]`
3. 列出可更新包 `yum list updates`
4. 列出已安装包 `yum list installed`
5. 列出已安装但不包含在资源库中的包 `yum list extras`

## repo

### 源的设置

```
$ cd /etc/yum.repos.d/
$ ls
CentOS-Base.repo  epel.repo.rpmnew   ius-archive.repo  ius.repo
epel.repo         epel-testing.repo  ius-dev.repo      ius-testing.repo

# 清除缓存
$ yum clean all

# 更新缓存
$ yum makecache
```

示例： [将Centos的yum源更换为国内的阿里云源](https://www.jianshu.com/p/4aa7b63f9026)

### 推荐源设置

一般来说，软件的源码安装、或者编译安装可以获得最新版本，但这样在服务器上，维护成本比较高。所以比较推荐的做法是使用第三方源

[CentOS配置IUS使用最新软件](https://www.yuzhi100.com/tutorial/centos/centos-peizhi-ius)

[Centos/RedHat 7/6/5切换阿里云源并安装EPEL/IUS/REMI仓库](https://blog.kuoruan.com/65.html)

#### 示例软件安装：git

使用第三方源，安装最新版 git

```
# 先移除系统软件
sudo yum remove git

# 安装 ius 源中的最新版软件
sudo yum install git2u
```

## Linux 翻墙

有时，需要在服务器命令行中执行安装或下载的命令，但会遇到翻墙的困扰，推荐安装完centos后，配置一下终端翻墙

### shadowsocks的安装

推荐使用客户端 shadowsocks-libev，因为 shadowsocks 客户端不支持 `aes-256-gcm` 方法。

[CentOS 7 下安装 Shadowsocks 服务端](https://zzz.buzz/zh/gfw/2017/08/14/install-shadowsocks-server-on-centos-7/)

>作为客户端使用，我们只需要用到 `ss-local` 命令

```
# 启动SS客户端
sudo su

nohup ss-local -c /etc/shadowsocks-libev/config.json >/home/xxx/log/shadowsocks-libev/sslocal.log 2>&1 &

## 开机自启
echo "nohup ss-local -c /etc/shadowsocks-libev/config.json >/home/xxx/log/shadowsocks-libev/sslocal.log 2>&1 &" >> /etc/rc.local

## 关闭 ss
pkill ss-local
```

>搭配 proxychains-ng 可实现终端翻墙

[Centos 7安装Proxychains实现Linux 代理](http://www.harker.cn/archives/proxychains.html)









