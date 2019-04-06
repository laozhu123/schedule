docker

### 一键安装docker
https://blog.csdn.net/dante_003/article/details/70208908

下载并安装docker
curl -sSL https://get.daocloud.io/docker | sh  -- 太慢了
**修改docker镜像地址，官方的镜像库连接太慢，这里转到daocloud镜像库。**
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://91c0cc1e.m.daocloud.io

### 启动docker服务，并设置开机启动
systemctl enable docker.service && service docker start

# docker
1.查找微服务运行的节点：

kubectl get pod -o wide -n default | grep price



2.进入容器：

sudo docker exec -it 容器名 /bin/bash



3.docker启动服务

systemctl enable docker.service && service docker start



4.docker查找镜像

docker search imagename



5.docker查看容器内部信息

sudo docker inspect gogsdb

