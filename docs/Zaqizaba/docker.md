
为了避免每次使用docker命令都需要加上sudo权限，可以将当前用户加入安装中自动创建的docker用户组(可以参考官方文档)：

`sudo usermod -aG docker $USER`

执行完此操作后，需要退出服务器，再重新登录回来，才可以省去sudo权限。

### 常规命令
```shell
# 启动命令
sudo systemctl start docker

# 停止
sudo systemctl stop docker

# 重启
sudo systemctl restart docker

# 修改配置后重启
sudo systemctl daemon-reload
sudo systemctl restart docker

# 查看版本
docker version

# 查看 Docker 信息
docker info

# Docker 帮助
docker --help
```

### 镜像命令

```shell
# 拉取一个镜像
docker pull ubuntu:20.04

# 列出本地所有镜像
docker images

# 删除镜像
docker images rm ubuntu:20.04
docker rmi ubuntu:20.04

# 创建镜像
docker [container] commit CONTAINER IMAGE_NAME:TAG

# 将镜像导出到本地
docker save -o ubunt_20_04.tar ubuntu:20.04

# 将镜像从本地文件中加载出来
docker load -i ubuntu_20_04.tar
```

### 容器（container）
```
# 利用镜像创建容器
docker [container] create -it ubuntu:20.04

# 查看本地所有容器
docker ps -a

# 启动容器
docker [container] start CONTAINER

# 停止容器
docker [container] stop CONTAINER

# 重启容器
docker [container] restart CONTAINER

# 创建并启动容器
dokcer [container] run -itd ubuntu:20.04

# 进入容器
docker [container] attach CONTAINER

# 先按 ctrl-p，再按 ctrl-q 可以挂起容器

# 在容器中执行命令
docker [container] exec CONTAINER COMMAND

# 删除容器
docker [container] rm CONTAINER

# 删除所有已停止的容器
docer container prune

# 将容器导出本地文件中
dokcer export -o xxx.tar CONTAINER

# 将本地文件 xxx.tar 导入成镜像，并将镜像命名为 image_name:tag
docker import xxx.tar image_name:tag

# docker export/import 会丢弃历史记录和元数据记录，仅保存容器当前的快照状态
# docker save/load 会保存完整记录，体积更大

# 查看某个容器内的所有进程
docker top CONTAINER

# 查看所有容器的统计信息，包括 CPU、内存、存储、网络
docker stats

# 在本地与容器间复制文件
docker cp xxx CONTAINER:xxx 或 dokcer cp CONTAINER:xxx xxx

# 重命名容器
docker rename CONTAINER1 CONTAINER2

# 修改容器限制
docker update CONTAINER --memory 500MB
```

### 参考资料

- https://cloud.tencent.com/developer/article/1848185