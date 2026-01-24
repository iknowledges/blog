# 常用Docker命令

```
# 查看镜像
docker images
# 拉取镜像
docker pull ubuntu:noble
# 删除镜像
docker rmi ubuntu:noble
# 运行一个容器
docker run --name my-ubuntu -itd -p 6789:22 ubuntu:noble
# 进入ubuntu操作界面
docker exec -it my-ubuntu bash
# 不进入容器执行ssh启动命令
docker exec -d my-ubuntu service ssh start
# 给已经运行的容器更新参数
docker update --restart always my-ubuntu
# 查看更新参数是否成功
docker inspect my-ubuntu --format '{{.HostConfig.RestartPolicy.Name}}'
# 查看运行的容器
docker ps -a
# 删除容器
docker rm my-ubuntu
# 启动容器
docker start my-ubuntu
# 停止容器
docker stop my-ubuntu
# 重启容器
docker restart my-ubuntu
```