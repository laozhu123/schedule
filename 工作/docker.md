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

