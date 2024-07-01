

# 深度学习服务器

均配备多块显卡，用来深度学习，内网穿透地址可以找我要
|id|内网ip（点进去可以查看gpu状态）|显卡|CPU|内存条|硬盘|内网穿透端口|
|--|--|--|--|--|--|--|
|1|172.29.196.140|3090x1，P102-10Gx4|E5-2698v3x2|64Gx4|1T SSD,16T HDD|5222|
|2|172.29.226.88|3090x1，P102-10Gx4|E5-2680v4x2|32Gx4|8T HDD|5223|
|3|172.29.206.144|3090x1，P102-10Gx4|E5-2680v4x2|32Gx4|8T HDD|5224|
|4|172.29.202.253|3090x1，P102-10Gx4|E5-2680v4x2|64Gx2|8T HDD|5225|
|5|172.29.197.178|3090x1，P102-10Gx4|E5-2680v4x2|64Gx2|16T HDD|5226|

# 添加用户
 
`sudo adduser [username]`

`sudo usermod -a -G adm,cdrom,sudo,dip,plugdev,lpadmin,lxd,sambashare,docker [username]`

`su [username]`

`echo 'export PATH=$PATH:/usr/local/cuda-12.1/bin' >> ~/.bashrc`

`echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-12.1/lib64' >> ~/.bashrc`

# 删除用户

`sudo deluser match --remove-home`

# 断网处理
有时候因为学校停电等原因，服务器断网超过24小时会导致需要网卡IP地址变更（使用以前的IP无法访问服务器）

1、去104机房，输入`ip address`就可以看到服务器的内网网址（172开头的）

2、打开浏览器访问`http://192.168.167.115/srun_portal_success`进行校园网认证，服务器才可以连上互联网。


<strike>

现在所有服务器都配备了显示器，所以下面的方法可以忽略了

但是咱们的服务器都没有接显示器，不方便看新ip地址。
这时候可以如果有一个usb网卡适配器(我的文件柜里面有一个powersync的，可以免驱)，就可以比较方便的去查看新ip地址。

1、先把usb网卡适配器插入校园网网线再插到自己的windows电脑上，这时候打开cmd看一下ipconfig

```Ethernet adapter Ethernet 5:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::1088:d86e:4279:ad1d%10
   IPv4 Address. . . . . . . . . . . : 172.29.35.42
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : 172.29.255.254
```
这里我们看到学校给我们这个网卡分配的ip是172.29.35.42

2、把该usb网卡适配器插服务器上，通过ssh到172.29.35.42上，用ip address命令查看服务器集成网卡的ip

3、拔掉该网卡适配器，通过X2GO登录上面看到的集成网卡的ip，再打开firefox，打开校园网认证url，`http://192.168.167.14/srun_portal_pc`认证之后就可以恢复上网（内网ip变成新的内网ip，内网穿透ip和端口不变）。
</strike>

## 远程桌面连接
自己电脑下载[X2GO](https://wiki.x2go.org/doku.php),新建连接时候注意session type选择XFCE（因为我给服务器装的都是轻量化的Xubuntu系统，默认XFCE桌面）

如果去校内的维修店给你装Ubuntu，默认就是KDE桌面

## docker的简单使用
- 创建容器
`docker run --gpus=all --runtime=nvidia --ipc=host --restart=always --privileged -tid --name=mmcv -p=50001:22 -v=/home/zhaolei/datasets:/home/zhaolei/datasets  -v=/home/zhaolei/Projects:/workspace -v=/home/zhaolei/.ssh:/root/.ssh  ufoym/deepo`
- 从host进入docker
`docker exec -i -t zhaolei /bin/bash`
- 修改端口映射
[看这里](https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-to-an-existing-docker-container)
- 查看运行的容器
`docker ps`
- 从容器复制到主机
`sudo docker cp 容器名:文件路径 主机文件/夹路径`
- 从主机复制复制到容器
`sudo docker cp 主机文件/夹路径 容器名:文件路径`
- 使用我设置的代理 `export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890` 
- 取消使用代理 `unset https_proxy` `unset http_proxy` `unset all_proxy` 

## 常用命令
- 查看GPU实时更新
`watch -n 1 nvidia-smi`
- 查看占用xxx端口的进程
`sudo lsof -i:xxx`
- 查看进程
`ps -ef`
- 杀死某个pid的进程
`sudo kill -9 [pid]`
- 查看资源图形界面
`alt`+`F2` `gnome-system-monitor`
- 赋予xxx文件夹所有权限
`chmod 777 -R xxx`
- 赋予xxx文件所有权限
`chmod 777 xxx`

## 显卡掉了怎么办
我们服务器有时候会出现这种情况

`(base) zhaolei@wls-team-server5:~$ nvidia-smi`

`NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.`

我们去[nvidia官网](https://www.nvidia.com/download/index.aspx)下载最新（之所以要最新，是因为我们的服务器经常自动更新Kernel，最新的驱动才能支持最新的Kernel）的Linux-X64,3090的驱动，比如NVIDIA-Linux-x86_64-550.78.run，安装步骤
- 安装之前：`sudo service lightdm stop`关闭图形化界面
- 安装：`sudo bash NVIDIA-Linux-x86_64-550.78.run`,一路选`Continue installation`，默认设置安装即可
- 安装之后：`sudo service lightdm start`打开图形化界面
