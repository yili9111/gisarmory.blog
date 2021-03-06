# 支持Canvas的Leaflet.Path.DashFlow动态流向线
通过对leaflet以及其插件的学习，我们了解到使用`Leaflet.Path.DashFlow`插件可实现轨迹动态展示、管道流向动态展示、河流流向动态展示等，达到增强可视化展示的效果。该插件使用方式非常简单，只需在正常添加线的时候，加入`dashArray`和`dashSpeed`参数即可。核心代码如下：

![202010100101](http://blogimage.gisarmory.xyz/202010100101.png)

> 注意，在`dashSpeed`为负时，线是正向流动。
>

效果如下：

![202010100101](http://blogimage.gisarmory.xyz/202010100101.gif)

但是在使用的过程中，发现该插件有个弊端，就是当使用`Canvas`方式绘制地图（初始化地图`preferCanvas`参数为`true`）时，动态效果不可用。那要如何解决这个问题呢？

通过对`Leaflet.Path.DashFlow`以及`leaflet`源码的研究，发现动态线的效果主要是通过动态刷新`dashOffset`参数的值，以改变线的样式来实现。

![202010190101](https://blogimage.gisarmory.xyz/202010190101.png)

`L.SVG`在`_updateStyle`的时候，更新了`dashOffset`参数，但是`L.Canvas`在`_updateStyle`时，并没有更新`dashOffset`参数。这即是在`Canvas`方式绘制时没有动态效果的原因。

L.SVG：

![202011090102](https://blogimage.gisarmory.xyz/202011090102.png)

L.Canvas：

![202011100103](https://blogimage.gisarmory.xyz/202011100103.png)

由此，我们找到了解决思路，即在`L.Canvas`的`_updateStyle`方法中，参考`L.SVG`的处理方式，添加对`dashOffset`参数的控制。核心代码如下：

![202010190104](https://blogimage.gisarmory.xyz/202010190104.png)



为方便使用，我们将`L.Path.DashFlow`插件重新封装，大家引用这个插件，即可在`Canvas`和`SVG`两种方式下使用此插件。



## 总结

1. 使用`Leaflet.Path.DashFlow`插件可实现轨迹动态展示、管道流向动态展示等动态线效果。
2. 默认插件在使用`Canvas`方式绘制地图（初始化地图`preferCanvas`参数为`true`）时，动态效果不可用。
3. 通过修改`L.Canvas`中代码，即可在`Canvas`方式时实现动态线效果。
4. 将`L.Path.DashFlow`插件重新封装，引用插件，即可在`Canvas`和`SVG`两种方式下实现动态线效果。

## 在线示例

[在线示例](
http://gisarmory.xyz/blog/index.html?demo=LeafletPathDashFlow)

[完整代码](
http://gisarmory.xyz/blog/index.html?source=LeafletPathDashFlow)

* * *

原文地址：[http://gisarmory.xyz/blog/index.html?blog=LeafletPathDashFlow](http://gisarmory.xyz/blog/index.html?blog=LeafletPathDashFlow)。

关注《[GIS兵器库](http://gisarmory.xyz/blog/index.html?blog=wechat)》公众号， 第一时间获得更多高质量GIS文章。

![](http://blogimage.gisarmory.xyz/20200923063756.png)

本文章采用 [知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议 ](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。欢迎转载、使用、重新发布，但务必保留文章署名《GIS兵器库》（包含链接：  [http://gisarmory.xyz/blog/](http://gisarmory.xyz/blog/)），不得用于商业目的，基于本文修改后的作品务必以相同的许可发布。


