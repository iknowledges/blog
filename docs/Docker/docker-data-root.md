# 修改Docker存储路径

1. 清除数据

```bash
# 删除没用的镜像
sudo docker image prune -a -f
# 删除没用的挂载
sudo docker volume prune -f
# 删除没用的容器和相应镜像
sudo docker system prune -a -f
```

2. 修改docker默认数据存储路径

```bash
sudo mkdir -p /home/roy/Desktop/docker/data
sudo vim /etc/docker/daemon.json
```

修改内容如下：
```
{
    "data-root":"/home/roy/Desktop/docker/data"
}
```

3. 重启docker服务

```bash
sudo systemctl restart docker
# 查看存储路径
sudo docker info -f '{{ .DockerRootDir }}'
```

## 参考资料

[How to Fix Docker’s No Space Left on Device Error](https://www.baeldung.com/linux/docker-fix-no-space-error)
