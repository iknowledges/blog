# KVM中CPU性能优化

查看Linux下CPU信息的命令：

```
lscpu
# 查看物理CPU，一个socket对应一个physical id
cat /proc/cpuinfo | grep "physical id" | sort | uniq
# 查看逻辑CPU
cat /proc/cpuinfo | grep "processor"
# 查看CPU的核数
cat /proc/cpuinfo | grep "cpu cores"| uniq
```

物理CPU的概念：
- 一个服务器可以有多个插槽(socket)，一个插槽可以插一个芯片(chip)。
- 一个芯片里面可以有多个核(core)。
- 一个核里面可以有1个CPU线程(thread)，如果开启超线程，CPU线程=核*2

总逻辑CPU数 = 物理CPU个数(socket) X 每颗物理CPU的核数 X 超线程数

操作系统在进程调度时为了保持亲和性，会优先把同一个进程调度到同一个core上，如果不能调度到同一个core，则尽量调度到同一个socket上。因此，如果想收获比较好的虚拟机性能表现，把虚拟机的CPU拓扑设置为和物理机一致，这样在亲和性保持上比较有利。

qemu-system-x86_64命令-smp参数：

- <格式>：-smp [cpus=]n[,maxcpus=cpus][,cores=cores][,threads=threads][,sockets=sockets]
- <子项>：
  - [cpus=]n 设置客户机中使用逻辑的CPU数量（默认值是1）。
  - [,maxcpus=cpus] 设置客户机最大可能被使用的CPU数量（可以用热插拔hot-plug添加CPU，不能超过maxcpus上限）。
  - [,cores=cores] 设置每个CPU socket上的core数量（默认值是1）。
  - [,threads=threads] 设置每个CPU core上的线程数（默认值是1）。
  - [,sockets=sockets] 设置客户机中总的CPU socket数量。

#### 参考资源

- [如何分配虚拟机CPU拓扑会得到较好的性能](https://mp.weixin.qq.com/s/ZtwKmG3xCsTShJni6xTCmw)
- [qemu-system-x86_64命令总结](http://blog.leanote.com/post/7wlnk13/%E5%88%9B%E5%BB%BAKVM%E8%99%9A%E6%8B%9F%E6%9C%BA)
- [QEMU User Documentation](https://www.qemu.org/docs/master/system/qemu-manpage.html)
