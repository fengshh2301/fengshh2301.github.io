---
title: centos-docker-setup
date: 2020-12-30 12:14:39
tags: [centos,docker]
categaries: [工具]
---

centos下安装docker<!-- more -->,最好使用阿里的镜像。

#### 1)安装docker

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

#### 2)修改docker配置

```bash
mkdir -p /etc/docker
cat > /etc/docker/daemon.json << __EOF__
{
  "registry-mirrors": ["https://siy3zhl4.mirror.aliyuncs.com"]
}
__EOF__
```

#### 3)设置docker目录

```bash
mkdir -p /home/dockerhub
vim /lib/systemd/system/docker.service
```

修改项目

```
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd --graph /home/dockerhub -H fd:// --containerd=/run/containerd/containerd.sock
```

#### 4)启动docker

```bash
systemctl daemon-reload
systemctl restart docker
systemctl enable docker
```

#### 5)查看docker是否安装成功

```bash
docker -v
docker images
docker ps -a
```

#### 6)获取docker镜像并运行

```bash
docker pull registry.cn-shanghai.aliyuncs.com/dataf/centos76:v4.1go1.15.6
```

使用以下命令运行镜像

```
docker run --name centos76V4.1 -p 80:80 -v /root/jason:/jason -it ffffffff /bin/bash
```

#### 7)其他相关docker命令

```
docker commit -a "author" -m "comment" ffffffff dir/centos76:v1
docker tag ffffffff registry.cn-shanghai.aliyuncs.com/dir/centos76:v1
docker login --username=xxx@yyy.com registry.cn-shanghai.aliyuncs.com
docker push registry.cn-shanghai.aliyuncs.com/dir/centos76:v1
```

