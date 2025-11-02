* 1 .概论：
  本文档主要帮助需要适配澜图的地图引擎了解澜图数据、服务管理结构与重要接口、以便适配。
* 2 .数据与服务架构
  澜图管理的数据可以分为：数据源、数据集（图层）、服务、缓存、样式。管理的数据可以分为，矢量数据、栅格数据、瓦片数据、三维数据。
  数据源支持：矢量数据库：包括PG、oracle（sdo）、mysql5.8、mysql8.0、人大金仓、翰高。文件数据源支持文件夹、共享文件夹、对象存储.
  服务涵盖：矢量地图服务、矢量查询服务、矢量瓦片服务、栅格地图服务、栅格瓦片服务、三维地形服务、三维模型服务。
* 3 .运行模式与接口简介：
  数据源、数据集、服务、样式有对应的接口，通过zookeep进行通知与管理。
* 3.1:数据源：
  获得列表：
  `  /apis/datasource/list?page=&rows=&orderBy=f_create_tim+desc`
  获得单体：
  `  /apis/datasource/getInfo/{id}`
  返回参数解析（没有说明则不需要用到，为系统逻辑字段）
  
  | 参数       | 值                                                                                                   |
| ---------- | ---------------------------------------------------------------------------------------------------- |
| createTime | 创建时间                                                                                             |
| id         | 主键                                                                                                 |
| name       | 名称                                                                                                 |
| user       | 用户名                                                                                               |
| password   | 加密后密码密码                                                                                       |
| status     | 状态 1：可用，0:不可用                                                                               |
| type       | 类型:oracle、postgreSQL、kingbase、mongodb、shape、fgdb、 raster、highgo、dameng、mysql、mysql8      |
| typeCode   | 01空间数据：0101矢量空间数据、0102矢量瓦片、0103栅格数据；02业务数据；03三维模型；04混数据源；05其他 |
  
  

监听节点：
zookeep中节点 `northpool_meta_root/vertex/datasource/`
key为对应的数据源的key value为最后更新时间
如果在该接口找不到对应的ID数据则为脏数据（有可能会存在脏数据情况）

* 3.2矢量数据集：
  获得列表
  `  /apis/dataservice/list?page=&rows=&type=1&orderBy=f_create_time+desc`
  获得单体
  ` /apis/dataservice/getInfo/{id}`
  返回参数解析（没有说明则不需要用到，为系统逻辑字段）

| 参数         | 值                                                                             |
| ------------ | ------------------------------------------------------------------------------ |
| center       | 中心点                                                                         |
| datasourceId | 数据源ID                                                                       |
| fields       | 可以使用的字段                                                                 |
| filter       | 过滤条件                                                                       |
| geotype      | 几何类型 POINT、MULTIPOINT、LINESTRING、POLYGON、MULTILINESTRING、MULTIPOLYGON |
| id           | ID                                                                             |
| isView       | 是否是试图                                                                     |
| keyField     | 主键ID                                                                         |
| name         | 名称                                                                           |
| shapeField   | 空间字段名称                                                                   |
| srText       | 空间参考porj_srs                                                               |
| srid         | srid                                                                           |
| tableName    | 表名                                                                           |
| type         | 数据类型，1 矢量数据、2 影像数据、3 地形数据                                   |
| updateTime   | 最后更时间                                                                     |
| xmax         | 四至                                                                           |
| xmin         | 四至                                                                           |
| ymax         | 四至                                                                           |
| ymin         | 四至                                                                           |

**监听节点：**
zookeep中节点 `northpool_meta_root/vertex/dataservice/`
key为对应的数据源的key value为最后更新时间
如果在该接口找不到对应的ID数据则为脏数据（有可能会存在脏数据情况）

* 3.3矢量地图服务：
  获得列表
  `/apis/services/list?page=1&rows=10&orderBy=&type=1`
  获得单体
  `/apis/services/getInfo?id={id}`
  返回参数解析（没有说明则不需要用到，为系统逻辑字段）

| 参数   | 值                  |
| ------ | ------------------- |
| layers | [组成服务的图层](#layers) |
| map    | [服务信息](#map)            |
| url    | [服务接口列表](#url)        |
| styles | [服务的样式列表](#styles) |

<span id="layers">layers 图层 解析：</layers>

| 参数         | 值                                                                             |
| ------------ | ------------------------------------------------------------------------------ |
| center       | 中心点                                                                         |
| datasourceId | 数据源ID                                                                       |
| fields       | 可以使用的字段                                                                 |
| filter       | 过滤条件                                                                       |
| geotype      | 几何类型 POINT、MULTIPOINT、LINESTRING、POLYGON、MULTILINESTRING、MULTIPOLYGON |
| id           | ID                                                                             |
| isView       | 是否是试图                                                                     |
| keyField     | 主键ID                                                                         |
| name         | 名称                                                                           |
| shapeField   | 空间字段名称                                                                   |
| srText       | 空间参考porj_srs                                                               |
| srid         | srid                                                                           |
| tableName    | 表名                                                                           |
| type         | 数据类型，1 矢量数据、2 影像数据、3 地形数据                                   |
| updateTime   | 最后更时间                                                                     |
| xmax         | 四至                                                                           |
| xmin         | 四至                                                                           |
| ymax         | 四至                                                                           |
| ymin         | 四至                                                                           |

<span id="map">map 服务 解析：</span>

| 参数        | 值                                                                                      |
| ----------- | --------------------------------------------------------------------------------------- |
| cacheConfig | [缓存配置](#cacheConfig)                                                                                |
| id          | id                                                                                      |
| name        | 名称                                                                                    |
| sid         | sid                                                                                     |
| srText      | 空间参考porj_srs                                                                        |
| srid        | srid                                                                                    |
| status      | 1为可用                                                                                 |
| styleName   | 默认样式                                                                                |
| tileSchema  | 网格方案，如果是-1则根据空间参考取空间参考默认的网格方案 默认网格方案按照国家天地图标准 |
| type        | 1 矢量服务 2 影像服务 3 地形服务 4 bundle瓦片服务 5 三维模型 6 xyz瓦片服务              |

<span id="cacheConfig">cacheConfig 缓存配置 解析</span>：

| 参数              | 值                                                                                                                                                        |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cacheDatasourceId | 缓存数据源ID                                                                                                                                              |
| dataCacheLevel    | 数据缓存级别                                                                                                                                              |
| dynamicUpdate     | 是否动态更新                                                                                                                                              |
| imgCacheLevel     | 图片缓存级别                                                                                                                                              |
| imgCacheMode      | 图标缓存模式 1是基本配置，2是手动配置 1为满足层级关系则缓存，2则取imgScript的判断规则                                                                     |
| imgScript         | 脚本判断规则 返回 true测创建缓存 语言为js 参数有serverName 服务名称, styleId 样式名称,level 层级,tileSize 瓦片大小, ratio 瓦片放大倍数 ,cost 生成瓦片时间 |
| lockImgCache      | 是否锁定图片缓存                                                                                                                                          |
| needDataCache     | 是否开启数据缓存                                                                                                                                          |
| needImgCache      | 是否开启图片缓存                                                                                                                                          |

imgScript样例

```imgScript样例
function(serverName, styleId,level,tileSize,ratio,cost){
  if(level > 16 && cost > 200 ){
    return true
  }else{
    return false
  }
}
```

当请求瓦片大于16级且创建瓦片消耗大于200ms则缓存

<span id="url">url 接口 解析：</span>

| 参数      | 值            |
| --------- | ------------- |
| id        | id            |
| name      | 描述          |
| serviceId | 服务id        |
| type      | 2 二维 3 三维 |
| url       | 地址          |

<span id="styles">styles 接口 解析</span>

| 参数       | 值                              |
| ---------- | ------------------------------- |
| content    | 样式描述，[详情见样式章节](#style) |
| id         | id                              |
| isDefault  | 是否默认样式1 默认              |
| levelRange | 层级范围                        |
| mapId      | 地图服务id                      |
| name       | 名称                            |
| sid        | sid                             |
| center     | 默认打开中心点                  |
| level      | 默认打开层级                    |
| xmax       | 默认打开四至                    |
| xmin       | 默认打开四至                    |
| ymax       | 默认打开四至                    |
| ymin       | 默认打开四至                    |

监听节点：
zookeep中节点` northpool_meta_root/vertex/vectorservice/`
key为对应的数据源的key value为最后更新时间
如果在该接口找不到对应的ID数据则为脏数据（有可能会存在脏数据情况）
temp_开头为临时样式，可以不需要监听 key为服务的sid

* 3.4影像地图服务：
  获得列表：
  ` /apis/dataservice/list?orderBy=&page=&rows=&type=2`
  获得单体：
  `/apis/dataservice/getInfo/{id}`
  影像服务获得单体解析

| 参数        | 值                                           |
| ----------- | -------------------------------------------- |
| cacheConfig | [缓存信息](#cacheConfig)                        |
| center      | 打开默认中心点                               |
| level       | 打开默认层级                                 |
| config      | [影像服务设置](#config影像服务设置)                                 |
| images      | [用到的影像数据](#images)                               |
| styleJson   | [样式](#styleJson)                                      |
| id          | id                                           |
| dataset     | [服务使用的影像数据集](#dataset)                         |
| name        | 名称                                         |
| srText      | srText                                       |
| srid        | srid                                         |
| type        | 数据类型，1 矢量数据、2 影像数据、3 地形数据 |
| url         | 服务接口列表                                 |

<span id="config影像服务设置">config  影像服务设置</span>

| 参数                      | 值                                                       |
| ------------------------- | -------------------------------------------------------- |
| terrain                   | 是否是地形                                               |
| bands                     | 通道                                                     |
| srid                      | srid                                                     |
| noDataValue               | 影像本身的noDataValue                                    |
| userNoDataValue           | 用户设置的 NoDataValue                                   |
| resampleConfigs           | [重采样设置](#resampleConfigs)                                               |
| dispelEdgedConfigs        | [去除边缘设置](#dispelEdgedConfigs)                                             |
| render                    | 指定通道，如果没有指定就为图像默认通道                   |
| grid                      | 网格，如果没有指定则根据srid设置网格，网格遵循天地图规范 |
| x                         | 打开默认中心点                                           |
| y                         | 打开默认中心点                                           |
| level                     | 打开默认层级                                             |
| contrastEnhancementConfig | [对比度增强](#contrastEnhancementConfig)                                               |

<span id="contrastEnhancementConfig">contrastEnhancementConfig</span>

| 参数               | 值                                                               |
| ------------------ | ---------------------------------------------------------------- |
| limits             | 1, 无增强 2, 最小最大值（默认） 3, 标准差方差范围 4 累计裁剪范围 |
| cumulativeCutLower | 累积裁切下限，默认值：0.02，仅在limits为4时有效                  |
| cumulativeCutUpper | 累积裁切上限，默认值：0.98 ，仅在limits为4时有效                 |
| stddevFactor       | 标准差影响因子，默认为：2 仅在limits为3时有效                    |

<span id="resampleConfigs">resampleConfigs</span>

| 参数          | 值                                                  |
| ------------- | --------------------------------------------------- |
| level         | 采用设置层级                                        |
| resampleRatio | 重采样倍数                                          |
| resampleMode  | 0:modeNearestNeighbour 1:modeBilinear 2:modeBicubic |

<span id="dispelEdgedConfigs">dispelEdgedConfigs</span>

| 参数              | 值                 |
| ----------------- | ------------------ |
| level             | 使用去除边缘的层级 |
| dispelPixelWidth  | 去除宽度           |
| dispelPixelHeight | 去除高度           |

<span id="images">images</span>

| 参数         | 值       |
| ------------ | -------- |
| img          | 影像名称 |
| datasourceId | 数据源id |

<span id="styleJson">styleJson</span>
为map结构

```结构
{"1-20":{
  "fileNameList":[
    "1961249099596623874-影像/深圳0.5.tif"
  ]
}
```

key 为层级 "1-20"为从1级到20级

| 参数         | 值                                                |
| ------------ | ------------------------------------------------- |
| fileNameList | array  层级使用的影像 格式为{数据源id}_{影像名称} |

<span id="dataset">dataset</dataset>

| 参数  | 值                 |
| ----- | ------------------ |
| files | array [使用影像信息](#files) |
| id    | 数据源id           |
| name  | 数据源名称         |

<span id="files">files</span>

| 参数            | 值                 |
| --------------- | ------------------ |
| bandCount       | 通道数             |
| bands           | 通道               |
| bbox            | 四至               |
| file            | 是否是文件         |
| hasPyramid      | 是否金字塔         |
| name            | 名称               |
| noDataValue     | 无效值             |
| path            | 绝对路径           |
| relativePath    | 相对数据源路径     |
| size            | 大小               |
| srid            | srid               |
| suitablePyramid | 金字塔层级是否合适 |

监听节点：
zookeep中节点 `northpool_meta_root/vertex/imageservice/`
key为对应的数据源的key value为最后更新时间
如果在该接口找不到对应的ID数据则为脏数据（有可能会存在脏数据情况）

* 4.地图样式
  <span id="style">地图样式可导出为mapbox样式格式</span>
  [mapbox样式解析]([Map Styles | Mapbox GL JS | Mapbox](https://docs.mapbox.com/mapbox-gl-js/guides/styles/))
  [mapbox样式指南]([Mapbox Style Spec | Mapbox Docs | Mapbox](https://docs.mapbox.com/style-spec/guides/))
  导出样式接口:
  ` /mapserver/styleInfo/{serviceName}/{styleName}/mapbox/style.json`
  [样例url](http://124.223.215.163:8021/mapserver/styleInfo/土地利用/土地利用/mapbox/style.json)
  [地图查看页面](http://124.223.215.163:8021/service-pub/#/services/map-preview/1961998547310276609/3/default)

监听节点：
zookeep中节点 `northpool_meta_root/vertex/imageservice/`
key为对应的数据源的key value为最后更新时间
如果在该接口找不到对应的ID数据则为脏数据（有可能会存在脏数据情况）

* 5: 测试地址
  [测试地址](#http://124.223.215.163:8021/portal/#/home)
  使用[用户名](http://northpool.top:8824/x1.txt)  登录后 [跳转地址](#http://124.223.215.163:8021/service-pub/#/datasource/vector)
  [zookeep测试地址](http://northpool.top:8824/x2.txt)
