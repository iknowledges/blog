# GeoServer教程

## 新建工作空间

1. 选择【工作空间】->【添加新的工作空间】
2. 填写名字和URI，点击保存

## 导入样式

1. 选择【样式】->【添加一个新的样式】
2. 填写名称，选择工作空间。
3. 【Upload a style file】，选择sld样式文件，点击上传。
4. 点击验证并保存。

## 导入shp文件

1. 选择【存储仓库】->【添加新的存储仓库】->【Shapefile】
2. 填写工作空间、数据源名称和文件位置，字符集选择UTF-8，点击保存并发布。
3. 选择【数据】，点击【从数据中计算】和【Compute from native bounds】
4. 选择【发布】，默认样式选择刚刚新建的样式，然后点击保存。

## 问题解决

- Leaflet渲染wms服务标注名称重复

在<se:TextSymbolizer>和<se:Label>之间填入下面内容：

```
<se:Geometry> 
    <ogc:Function name="centroid">
    <ogc:PropertyName>the_geom</ogc:PropertyName>
    </ogc:Function>
</se:Geometry>
```