# Linux服务器

均配备多块显卡，用来深度学习，内网穿透地址可以找我要
|id|内网ip|显卡|CPU|内存条|硬盘|内网穿透端口|
|--|--|--|--|--|--|--|
|1| 172.24.230.170 |3090✖1，P102-10G✖5|E5-2698v3✖2|64G✖4|1T SSD,16T HDD|5222|
|2|172.24.26.53|3090✖1，P102-10G✖5|E5-2680v4✖2|32G✖4|8T HDD|5223|
|3|172.24.34.140|3090✖1，P102-10G✖5|E5-2680v4✖2|32G✖4|8T HDD|5224|
|4|172.24.162.214|3090✖1，P102-10G✖5|E5-2680v4✖2|64G✖2|8T HDD|5225|
|5|172.24.91.3|3090✖1，P102-10G✖5|E5-2680v4✖2|64G✖2|16T HDD|5226|
|6|172.24.235.12|P102-10G✖6|E5-2683v3✖2|32G✖3，8G✖4|4T HDD|5227|

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

# Windows服务器
用来存放数据集
172.24.239.247

