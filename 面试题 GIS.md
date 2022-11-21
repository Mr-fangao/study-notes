面试题 

一、GIS部分

1. 地理坐标和投影坐标的区别

   地理坐标系统，是以经纬度为地图的存储单位的，是球面坐标系统

   投影坐标系统，实质上便是平面坐标系统，其地图单位通常为米

2. 常用的投影有哪些

   等角正切圆柱投影(墨卡托投影)

   Gauss-Krüger(等角横切椭圆柱投影)

   UTM投影（Universal Transverse Mercator Projection--通用横轴墨卡托投影，横轴等角割椭圆柱面投影）

3. 常用的地理坐标系统

   CGCS2000、西安80、北京54、WGS84

4. WMTS、WMS、WFS是什么

   Web地图服务（WMS）利用具有地理空间位置信息的数据制作地图，GetCapabilities返回服务级[元数据](https://so.csdn.net/so/search?q=元数据&spm=1001.2101.3001.7020)，它是对服务信息内容和要求参数的一种描述； GetMap返回一个地图影像，其地理空间参考和大小参数是明确定义了的；GetFeatureInfo（可选）返回显示在地图上的某些特殊要素的信息

   WMTS，切片地图Web服务（OpenGIS Web Map Tile Service），提供了一种采用预定义图块方法发布数字地图服务的标准化解决方案

   Web矢量服务（WFS）返回的是矢量级的GML编码，并提供对矢量的增加、修改、删除等事务操作

5. 矢量和栅格的区别

   矢量由点的坐标构成，结构紧凑精度高可放大

   栅格数据是由很多小方块像素组成的图形，数据量大

6. Geojson

   GeoJSON是一种用于编码各种地理数据结构的格式。GeoJSON对象可以表示几何形状，特征或特征集合。

7. 基于WGS84的常用坐标系为两种

   - EPSG:4326
   - EPSG:3857

   4326是地理坐标系，以度为单位。广泛用于GPS定位等。

   3857是投影坐标系，以米为单位。用于平面地图。

8. 切片原理

   将空间数据以影像金字塔的形式渲染为不同缩放级别的地图图片，然后将各个级别的图片按照一定规则切分，当缩放级别不同的时候，显示不同级别的切片数据

   1、切片原点

   切片原点一般有两种：1、左上角；2、左下角。大部分切片的算法是采用左上角作为切片原点的，例如天地图、Arcgis Server等等。切片原点定义了我的第一个 256*256的切片的位置。

   2、切片分辨率（比例尺）

    切片分辨率和比例尺是一一对应的。比例尺指的是我的图上一个单位代表实际多少距离，而分辨率代表屏幕上一个像素代表实际多少距离，因此，当定了一个之后，对应的另一个也确定了。

   切片分辨率（比例尺）影响的是在该分辨率（比例尺）下，我的一个256*256的图片的实际坐标是多少。

   3、初始化切片范围

   初始化切片范围是指我是按照多大的图幅进行切片的，影像的是我在这个分辨率下总共能切多少个256*256的切片。

9. 3Dtiles有几种数据格式

   Batched 3D Model (b3dm)异构三维模型。例如，纹理地形和表面，3D建筑内外，大型模型。

   Instanced 3D Model (i3dm)实例化3D模型(i3dm)。树木、风车、螺栓。

   Point Cloud (pnts)点云(pnts)大量的点。

   Composite (cmpt)复合(cmpt)将不同格式的瓦片连接成一个瓦片。

10. Arcgis常用API

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105161007502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxODE5MQ==,size_16,color_FFFFFF,t_70)

11. surpermap三维客户端 

    cesium加载影响  CesiumTerrainProvider地形 SuperMapImageryProvider影像服务

    场景颜色设置 colorCorrection 泛光效果开关 bloomEffect  绘制线DrawHandler

12. 加载模型并改变位置

     model = scene.primitives.add(
        Cesium.Model.fromGltf({

    空间位置的移动---将经纬度和高度转换为笛卡尔坐标 Cesium.Cartesian3.fromDegrees

    构建模型矩阵---通过姿态变化、位置变化构建模型矩阵 Cesium.Transforms.headingPitchRollToFixedFrame

13. 苏州园区 mapbox加geoserve的wms服务

14. 天地图坐标系：CGCS2000

15. 

    

二、Web部分

1. axios和ajax的区别

    Ajax是对原生XHR的封装,由客户端请求ajax引擎，再由ajax引擎请求服务器，服务器作出一系列响应之后返回给ajax引擎，由ajax引擎决定将这个结果写入到客户端的什么位置。实现页面无刷新更新数据。

      Axios 是一个基于 Promise 的 HTTP 库，可以用在浏览器和 node.js 中

2. 如何用flex实现居中

   justify-content: center;   */\* 子元素水平居中 \*/*        

   align-items: center;       */\* 子元素垂直居中 \*/*

3. localstroge储存数据能不能跨域访问

   A域名向中间页面C发送存储数据，C页面进行数据存储，B域名页面向C页面发送请求读取存储数据

4. vue中的跨域问题如何解决

   **在vue中使用proxy进行跨域的原理是：将域名发送给本地的服务器（启动vue项目的服务,loclahost:8080），再由本地的服务器去请求真正的服务器。**

   在vue.config.js中设置如下代码片段

   ```javascript
   module.exports = {
     dev: {
       // Paths
       assetsSubDirectory: 'static',
       assetsPublicPath: '/',
       proxyTable: { // 配置跨域
       '/api':{
           target:`http://www.baidu.com`, //请求后台接口
           changeOrigin:true, // 允许跨域
           pathRewrite:{
               '^/api' : '' // 重写请求
           }
       }
     },
   }
   ```

   创捷axioss实例时，将baseUrl设置为 ‘/api’

5. css中各种选择器的权重排行

   ！important      权重为：          infinity

   行间样式          权重为：           1000

   id 选择器              权重为：       100

   class|属性|伪类       权重为：       10

   标签(html标签)|伪元素            权重为：         1

   通配符               权重为：               0

6. 盒子模型 如何不用border实现边框

   box-shadow: 0 0 0 5px #000;

7. settimeout为0时会发生什么

   js是单线程，所有任务都需要排队，设置会0时，任务会最后执行

8. vue3的特性差

   1、组合式API---setup
   在vue3中：setup是一个接收 props 和 content 的函数，允许我们在其内部通过API的方式（如：onMounted(getUserRepositories) // 在 `mounted` 时调用 `getUserRepositories`）多次使用一个相同的生命周期等。
   2、ref创建响应式数据
   3、Teleport---“传送门”
   允许我们将指定内容渲染在指定的html标签上
   4、多根节点
   在vue2中：一旦根节点有多个，vue会发出警告
   在vue3中：支持多根节点（减少了DOM元素的嵌套层级）
   5、style中使用变量

9. get和post区别

   GET把参数包含在URL中，POST通过request body传递参数

   GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；

   而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok

10. 闭包

    闭包就是**在一个函数内部创建另一个函数**，让你可以在一个内层函数中访问到其外层函数的作用域。又或者说，**闭包就是能够读取其他函数内部变量的函数**。

    ```javascript
    function f1(){
        var n=2;
        function f2(){
            var q=0;
            console.log('n=',++n);
            console.log('q=',++q);
        }
        return  f2;
    }
    
    var f=f1();
    f() // 3 1
    f() // 4 1
    ```

    

11. webpack、babel是干什么的

     webpack把项目当做一个整体, 通过设定的入口文件, 从该文件开始找到项目的所有依赖文件, 使用loaders处理它们, 最终打包成一个或多个js文件.

     babel是将较新版本的js, 转化成大多数浏览器都可识别的老版本的语法的工具.

12. es6新特性

13. 1

14. 1

15. 1