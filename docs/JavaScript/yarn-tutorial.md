# yarn教程

## 安装yarn

```
npm install -g yarn
```

## 常用命令

```
# 安装项目所有依赖
yarn install
# 启动项目（run可以省略）
yarn run start
# 打包项目
yarn run build
# 安装某个包
yarn add [packages ...]
```

## 问题解决

- yarn安装依赖卡在Building fresh packages...

在项目根目录新建.yarnrc文件，内容如下：

```
registry "https://registry.npm.taobao.org"

sass_binary_site "https://npm.taobao.org/mirrors/node-sass/"
phantomjs_cdnurl "http://cnpmjs.org/downloads"
electron_mirror "https://repo.huaweicloud.com/electron/"
sqlite3_binary_host_mirror "https://foxgis.oss-cn-shanghai.aliyuncs.com/"
profiler_binary_host_mirror "https://npm.taobao.org/mirrors/node-inspector/"
chromedriver_cdnurl "https://cdn.npm.taobao.org/dist/chromedriver"
```