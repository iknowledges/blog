# Nginx Proxy Manager使用教程

## 安装Docker

1. 卸载老版本

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

2. 设置仓库

```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

3. 安装Docker Engine

```
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# 启动docker
sudo systemctl start docker
```

## 安装Nginx Proxy Manager

1. 创建一个docker-compose.yml文件

```
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

2. 操作容器

```
# 启动容器
docker compose up -d
# 关闭容器
docker compose down
```

3. 访问http://127.0.0.1:81，默认用户如下：

```
Email:    admin@example.com
Password: changeme
```

#### 参考资源

- [Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/#install-docker-engine)
- [Nginx Proxy Manager](https://nginxproxymanager.com/guide/#quick-setup)

