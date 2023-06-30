# Windows服务器
用来存放数据集、下载东西（有百度网盘年费vip）和打印文件，账号密码可以从我要
存放在该服务器D盘datasets目录下的所有东西，都可以通过linux服务器的/datasets目录访问
172.24.239.247

# Linux服务器

均配备多块显卡，用来深度学习，内网穿透地址可以找我要
|id|内网ip|显卡|CPU|内存条|硬盘|内网穿透端口|
|--|--|--|--|--|--|--|
|1| 172.24.230.170 |3090x1，P102-10Gx4|E5-2698v3x2|64Gx4|1T SSD,16T HDD|5222|
|2|172.24.26.53|3090x1，P102-10Gx4|E5-2680v4x2|32Gx4|8T HDD|5223|
|3|172.24.34.140|3090x1，P102-10Gx4|E5-2680v4x2|32Gx4|8T HDD|5224|
|4|172.24.162.214|3090x1，P102-10Gx4|E5-2680v4x2|64Gx2|8T HDD|5225|
|5|172.24.222.248|3090x1，P102-10Gx4|E5-2680v4x2|64Gx2|16T HDD|5226|
|6|172.24.102.114|P102-10Gx6|E5-2683v3x2|32Gx4|4T HDD|5227|

## 添加用户
 
`sudo adduser [username]`

`sudo usermod -a -G adm,cdrom,sudo,dip,plugdev,lpadmin,lxd,sambashare,docker [username]`

`su [username]`

`echo 'export PATH=$PATH:/usr/local/cuda-12.1/bin' >> ~/.bashrc`

`echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-12.1/lib64' >> ~/.bashrc`


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

## 常用命令
- 查看GPU实时更新
`watch -n 1 nvidia-smi`
- 查看占用xxx端口的进程
`sudo lsof -i:xxx`
- 查看进程
`ps -ef`
- 杀死某个pid的进程
`kill -9 pid`
- 查看资源图形界面
`alt`+`F2` `gnome-system-monitor`


