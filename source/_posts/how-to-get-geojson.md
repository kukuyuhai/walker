---
title: 使用openstreetmap获取geojson数据
date: 2021-6-18 10:46:20
tags: osm geojson webgl 3d可视化
---

> 城市 3D 可视化。随着 WebGL、物联网、GIS 等相关科学技术的飞速发展，越来越多的城市开始数字化转型。运用数字孪生技术，打造智慧城市场景。更好的服务城市建设治理。

### 1. 数据获取

- 打开 https://www.openstreetmap.org/ 搜索城市名称如"上海市"，点击下载 XML。

```XML
  <relation id="398353">
```

- 找到城市的边界 id，将 id 加上 3600000000 后，得到的结果用于后续`Overpass API query `请求的参数。

- 打开 http://www.overpass-api.de/query_form.html 访问 API query page,在打开的 query 框中输入以下代码：

```xml

<osm-script timeout="1800" element-limit="100000000">
 <union>
  <area-query ref="3600398353"/>
  <recurse type="node-relation" into="rels"/>
  <recurse type="node-way"/>
  <recurse type="way-relation"/>
 </union>
 <union>
  <item/>
  <recurse type="way-node"/>
 </union>
 <print mode = "body"/>

```

- 将`<area-query ref="xxxxxxx">`中的 ref 的值替换成之前相加后的 relation id 的值

- 点击右下角的 query，弹出下载提醒，单击保存；下载完成后，以记事本方式打开该文件（此文件即 上海市 的城市地图数据），导入到仿真软件中使用即可

### 2. 数据处理

- 将下载的文件，修改其后缀名为`.osm`,可以使用文本编辑器打开，其实它是一个 XML 文件，如何将 XML 转换成我们后续需要 geojson 呢？我们可以使用 osmtogeojson 包转换。如下全局安装 osmtogeojson。

```sh
  $ npm install -g osmtogeojson
```

```sh
  $ osmtogeojson file.osm > file.geojson
```

即可转换生成 geojson 数据。

#### _相关链接：_

[OpenStreetMap 开放的地理信息数据库](https://www.openstreetmap.org/)

[overpass Api](http://www.overpass-api.de/query_form.html)
