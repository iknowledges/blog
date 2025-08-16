# drawio开发笔记

项目地址：https://github.com/jgraph/drawio

## 打开源码
1. 使用idea打开项目
2. 打开文件->项目结构->Facet，添加web项目，选择drawio模块
3. 编辑部署描述符，路径选择/drawio/src/main/webapp/WEB-INF/web.xml文件，web资源路径改成/drawio/src/main/webapp路径
4. 如果提示Create Artifact，就点击创建

## 使用ant编译
1. Ant标签页下添加/drawio/etc/build/build.xml

## 使用Tomcat运行
1. 添加tomcat运行配置，在部署下添加刚刚创建的Artifact
2. 配置热部署：
- On 'Update' action: 更新类和资源
- On frame deactivation: 更新类和资源

## 火狐浏览器开启读取本地文件权限
1. 点击打开火狐浏览器，在地址栏输入：about:config；
2. 弹出提示，点击我了解风险；
3. 输入security.fileuri.strict_origin_policy，把这个值改为false。

## 调试设置

&dev=1&test=1
index.html

urlParams获取url参数组装成对象
创建两个meta标签apple-mobile-web-app-title和application-name

加载js文件：
/js/PreConfig.js
/js/diagramly/Init.js
/js/grapheditor/Init.js
/mxgraph/mxClient.js
/js/diagramly/Devel.js
/js/PostConfig.js
