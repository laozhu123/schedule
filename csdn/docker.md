# docker
### 一键安装docker
https://blog.csdn.net/dante_003/article/details/70208908

下载并安装docker
curl -sSL https://get.daocloud.io/docker | sh  -- 太慢了
**修改docker镜像地址，官方的镜像库连接太慢，这里转到daocloud镜像库。**
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://91c0cc1e.m.daocloud.io

### 启动docker服务，并设置开机启动
systemctl enable docker.service && service docker start
