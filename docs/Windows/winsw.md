# WinSW打包java服务

1. 下载最新版的[WinSW-x64.exe](https://github.com/winsw/winsw/releases)
2. 将WinSW-x64.exe重命名为myapp.exe，并在同一目录下新建myapp.xml，内容如下：

```xml
<service>
  <id>myapp</id>
  <name>myapp</name>
  <description>This service runs Jenkins continuous integration system.</description>
  <env name="JENKINS_HOME" value="%BASE%"/>
  <executable>java</executable>
  <arguments>-Xrs -Xmx256m -jar "%BASE%\jenkins.war" --httpPort=8080</arguments>
  <log mode="roll"></log>
  <logpath>"%BASE%/logs/</logpath>
</service>
```

3. cmd执行如下命令将myapp注册为Windows服务，并启动服务：

```
myapp.exe install
myapp.exe start
```

4. 停止并删除Windows服务

```
myapp.exe stop
myapp.exe uninstall
```

#### 参考资料：

[WinSW](https://github.com/winsw/winsw)
