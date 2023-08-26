# 开发板挂载 Ubuntu 的 NFS 目录

**什么是 NFS 协议？**

> NFS（Network File System，网络文件系统）是众多文件共享协议之一

NFS 实现了一个跨越网络的文件访问功能，如下图可以简要说明其原理。其 整个架构为 Client-Server 架构，客户端和服务端通过 RPC 协议进行通信，RPC 协议可以简单的理解为一个基于 TCP 的应用层协议，它简化命令和数据的传输。 NFS 最大的特点是将服务端的文件系统目录树映射到客户端，而在客户端访问该 目录树与访问本地文件系统没有任何差别，客户端并不知道这个文件系统目录树 是本地的还是远在另外一台服务器。

![NFS System](./../../../Users/meng.wang/Documents/CS%E4%B9%8B%E8%B7%AF/self-taught-experience/imgs/NFS%20System.png)

**我们为什么要挂载 ubuntu 的 nfs 目录？** 

我们有些时候需要多次调试开发板文件系统内的某个应用程序，这就需要多 次进行编译拷贝等操作，所以我们在前期进行调试时可以直接让开发板使用 ubuntu 的 nfs 目录下文件系统来进行远程调试，用以提高调试效率，加快研发 速度。

## 确定 ubuntu 的 IP

开发板要想访问 Ubuntu,要先确定 ubuntu 的 IP：

![get ubuntu ip](./../../../Users/meng.wang/Documents/CS%E4%B9%8B%E8%B7%AF/self-taught-experience/imgs/get_ubuntu_ip.png)

## NFS 的使用

### 服务端

``` bash
# 1. 检查 ubuntu 是否安装 nfs 服务
~$ dpkg -s nfs-kernel-server
# 2. 安装 nfs 服务
~$ sudo apt-get install nfs-kernel-server
# 3. 创建挂载点
~$ mkdir items
# 4. 配置挂载点
~$ sudo vim /etc/exports
# 格式：
# [挂载点] [可以访问的IP]([权限]) *: 代表所有 IP 都允许访问
 /home/wm/items *(rw,nodide,insecure,no_subtree_check,async,no_root_squash)
# 保存修改并导出共享，每次修改都要执行
~$ sudo exportfs -ar
# 5. 启动 nfs 服务和 rpcbind 服务
~$ systemctl start nfs-server
~$ systemctl start rpcbind
# 6. 给挂载点授权
sudo chown -R wm.wm /home/wm/items
```

### 客户端

已经获知 ubuntu 的 IP 是 192.168.1.7，确保开发板能够 ping 通 ubuntu 后，在开发板上执行以下命令挂载 nfs：

``` bash
[root@100ask:~]# mount -t nfs -o nolock,vers=3 192.168.1.7:/home/wm/items /mnt
```

mount 命令用来挂载各种支持的文件系统协议到某个目录下。

mount 成功之后，开发板在/mnt 目录下读写文件时，实际上访问的就是 Ubuntu 中的/home/book/nfs_rootfs 目录，所以开发板和 Ubuntu 之间通过 NFS 可以很方便地共享文件。 在开发过程中，在 Ubuntu 中编译好程序后放入/home/book/nfs_rootfs 目录，开发板 mount nfs 后就可以直接通过/mnt 访问 Ubuntu 中的文件。

