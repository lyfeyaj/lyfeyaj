介绍一款 Ubuntu 虚拟机管理神器 - Multipass
====================================

## 一. Multipass 是什么？

Multipass 是一个轻量级 Linux 虚拟机命令行管理工具，支持 Linux、Windows 与 macOS。


## 二. 为什么要用 Multipass

1. 能够以最小的成本和资源在本地快速搭建具备完整 Ubuntu 功能小型虚拟机集群（如测试 K8s各类特性、数据库小集群等）
2. 可以方便快速的做各类 Linux 试验，而不用担心把系统搞坏，重建一个新系统只要几分钟
3. 实例通过命令行管理，对开发非常友好，每个实例IP固定

## 三. 如何安装

### Mac OS 安装

#### 方法一：可以直接下载安装包安装

[点击下载 Multipass 安装包](https://multipass.run/download/macos)

#### 方法二：使用 Homebrew 安装

> 没有安装 Homebrew? [点击这里安装 Homebrew](http://brew.sh/)

```bash
brew install multipass
```

### Linux 安装

使用 Snapcraft 安装

> 没有安装 Snapcraft? [点击这里安装 Snapcraft](https://snapcraft.io/docs/installing-snapd)

```bash
sudo snap install multipass
```


### Windows 10 安装

#### 方法一：可以直接下载安装包安装

[点击下载 Multipass 安装包](https://multipass.run/download/windows)

#### 方法二：使用 Chocolatey 安装

> 没有安装 Chocolatey? [点击这里安装 Chocolatey](https://chocolatey.org/install)

```bash
choco install multipass
```

## 四. 功能介绍

可在 [Multipass 官网](https://multipass.run/) 查看详细使用文档。

```
-> ~ $ multipass help
用法: multipass [options] <command>
创建, 控制和连接 Ubuntu 实例。

multipass 命令行工具, 用于管理 ubuntu 实例。

参数:
  -h, --help     查看本帮助内容
  -v, --verbose  增加日志显示的详细程度。 通过在短参数中增加 'v' 来获取更多日志信息
                 最多支持4个等级，如： -vvvv。

可用的命令:
  delete    删除实例
  exec      在实例中执行命令
  find      查找并列出可用于创建实例的镜像
  get       获取某个配置项
  help      查看帮助
  info      查看实例信息
  launch    创建并启动实例
  list      列出所有实例
  mount     挂载文件夹到实例
  purge     清除已删除的实例
  recover   恢复已删除的实例
  restart   重启实例
  set       设置某个配置项
  shell     通过 shell 连接实例
  start     启动实例
  stop      停止实例
  suspend   挂起实例
  transfer  在本机和实例之间传输文件
  umount    移除实例中挂载的文件夹
  version   查看版本号
```

## 五. 常见问题（以 MacOS 为例）

### 问题一：最开始设置的内存或 CPU 数量小了，想扩容，怎么办？

multipass 通过 `/var/root/Library/Application\ Support/multipassd/multipassd-vm-instances.json` 中的配置来管理实例，可直接在这个配置文件中修改：
`mem_size` 来增加或减少内存
`num_cores` 来增加或减少CPU核心数

修改之前需要先停止 multipass 的进程，原因是 multipass 会在被关闭的时候将各个实例的状态写入到配置文件，所以在没有关闭 multipass 进程的时候修改配置文件，会被覆盖。

```bash
# 停止 multipassd 进程
sudo launchctl unload /Library/LaunchDaemons/com.canonical.multipassd.plist

# 编辑 /var/root/Library/Application\ Support/multipassd/multipassd-vm-instances.json 文件
# 需要 root 权限

# 重新启动 multipassd 进程
sudo launchctl load /Library/LaunchDaemons/com.canonical.multipassd.plist
```

### 问题二：电脑意外关机，无法启动实例，怎么办？

实例的启动关闭状态也维护在 `/var/root/Library/Application\ Support/multipassd/multipassd-vm-instances.json` 文件中的 `state` 字段，当电脑意外关机，`state` 字段不会被正确的维护，导致无法启动或关闭实例，这时候，可以先停止 multipassd 进程，然后手动到配置文件中修改 `state` 为 0，即关机状态，保存配置文件，并启动 multipassd 实例即可，这时候就可以正常启动各个实例了。

### 问题三：能安装 Cent OS 实例么？
暂时不能，该工具为 Ubuntu 背后的公司 Canonical 开发，目前仅支持 Ubuntu 系统。

### 问题四：如果在实例之间传递文件？
最简单的方式是通过挂载相同的文件夹到不同的实例中来共享文件。

## 参考

1. [multipass 官网](https://multipass.run/)