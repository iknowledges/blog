# 什么是聚集索引

聚集索引clustered index也叫聚簇索引，它的定义是：聚集索引的表中数据行的物理顺序与列值（一般是主键的那一列）的逻辑顺序相同，一个表中只能拥有一个聚集索引。

![clustered-index](https://gitlab.com/iknowledge/BlogImage/-/raw/main/MySQL/clustered-index.jpg)

结合上面的表格就很好理解了：数据行的物理顺序与列值的顺序相同，如果我们查询id比较靠后的数据，那么这行数据的地址在磁盘中的物理地址也会比较靠后。聚集索引存储记录是物理上连续存在，而非聚集索引是逻辑上的连续，物理存储并不连续。