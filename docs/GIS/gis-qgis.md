# QGIS教程

## 数据源

- [阿里云地图选择器](http://datav.aliyun.com/portal/school/atlas/area_selector)
- [全国地理信息资源目录服务](https://www.webmap.cn/commres.do?method=dataDownload)
- [OpenStreetMap数据镜像](http://download.geofabrik.de/)

以阿里云地图选择器为例：选择【范围选择器】，在地图上选择一个范围，然后点击【其他类型】中的下载按钮，下载json文件。

## QGIS数据处理

1. 打开【Data Source Manager】->【Vector】->【File】，在【Vector Dataset】选择刚刚下载的json文件，然后点击添加。
2. 右键刚刚添加的图层，点击【Export】->【Save Features As】，在【Select fields to export and their export options】中将JSON类型的选项去掉，然后设置文件名点击确定，即可导出shp文件。

## QGIS样式处理

1. 右键图层，点击【Properties】
2. 【Symbology】->【Symbol layer type】，选择【Outline:Simple Line】
3. 【Labels】->【Single Labels】
4. 【Rendering】->【Scale Dependent Visibility】设置相机高度以控制显示样式的范围。
5. 导出样式：【Style】->【Save Style】，选择SLD Style File，然后点击确定导出文件。
