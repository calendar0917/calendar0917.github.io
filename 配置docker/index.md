# 配置docker


## 查看是否已安装

```bash
docker --version
```

若已安装：

```bash
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine \
                  docker-ce

```

## 安装依赖工具 yum

```bash	
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

> 错误：yum-config-manager：找不到命令
>
> yum -y install yum-utils

> 错误：更新 yum 报错
>
> sudo tee /etc/yum.repos.d/CentOS-Base.repo <<-'EOF'
> [base]
> name=CentOS-$releasever - Base
> baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
> gpgcheck=1
> gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
>
> [updates]
> name=CentOS-$releasever - Updates
> baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
> gpgcheck=1
> gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
>
> [extras]
> name=CentOS-$releasever - Extras
> baseurl=http://mirrors.aliyun.com/centos/$releasever/extras/$basearch/
> gpgcheck=1
> gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
>
> EOF

## 安装docker

添加 docker 官方仓库

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

安装

```bash
bash sudo yum install -y docker-ce docker-ce-cli containerd.io
```

启动、设置开机自启

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### docker 拉取镜像源配置

添加多个镜像源

```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://alzgoonw.mirror.aliyuncs.com",
    "https://docker.m.daocloud.io",
    "https://dockerhub.icu",
    "https://docker.anyhub.us.kg",
    "https://docker.1panel.live"
  ]
}

EOF
```

重新加载并重启

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

测试

```bash
docker pull hello-world
```


