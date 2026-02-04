# docker上传镜像到仓库

1. 先到[docker hub](https://hub.docker.com/repositories)创建仓库，这里假设创建的仓库名为ubuntu
2. 可以通过两种方式创建本地镜像：
- 从现有镜像创建
```
docker tag ubuntu:bionic {username}/ubuntu:bionic
```
- 从容器创建
```
docker commit my-ubuntu {username}/ubuntu:bionic
```
3. 推送到远程仓库
```
docker login -u {username} -p {password}
docker push {username}/ubuntu:bionic
docker logout
```