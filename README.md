# resources
资源仓库
# live2dw的使用 导入
在页面导入<script src="live2dw/lib/L2Dwidget.min.js"></script>
# 初始化代码 注意路径别写错了就行
    L2Dwidget.init({
				pluginRootPath: "live2dw/",//资源root路径
				pluginJsPath: "lib/",//js相对root的路径
				pluginModelPath: "assets/",//模型相对root的路径
				tagMode: !1,
				debug: !1,
				model: {
					scale: 2,
					jsonPath: "live2dw/assets/asuna_33.model.json"
				},
				display: {
					position: "left",//定位
					width: 130,//宽度
					height: 210,//高度
					hOffset: -40,//左右
					vOffset: 0//上下
				},
				mobile: {
					show: !1 //1为移动端不显示，调为0即可显示
				},
				log: !1
			});

# mybatis逆向工程的使用
在generatorConfig.xml文件中配置好数据库连接信息以及要生成的数据表和代码存放位置之后
运行GeneratorSqlMapper.java的主方法
运行主方法之后刷新src目录即可看到生成的代码了~
如果再配置个mybatis的嵌套查询，封装一下结果集就更舒服了


# 前端中的Live2D与技术

![](https://ask8088-private-1251520898.cn-south.myqcloud.com/developer-images/article/3335308/ydd2m5u7w9.png?q-sign-algorithm=sha1&q-ak=AKID2uZ1FGBdx1pNgjE3KK4YliPpzyjLZvug&q-sign-time=1594734118;1594741318&q-key-time=1594734118;1594741318&q-header-list=&q-url-param-list=&q-signature=b08902271e6c387c3d572e3838cec9574fd4aca8)

Live2D，即为2D绘图渲染技术，游戏中例如崩坏3，碧蓝航线中与老婆交互就利用了live2d，通过前端的渲染，拼接，移动即可做出类似3d的效果，但是始终还是有缺陷，例如无法实现3d的转身，跳跃。

L2D的制作成本远低于3D，3D需要的建模，渲染，L2D可以方便完成，只需要对人物切片，然后前端重组，移动动画即可达到效果

# Web中的L2D结构

案例：[www.arsrna.com](https://www.arsrna.com)

这是Live2D的材质结构

![](https://ask8088-private-1251520898.cn-south.myqcloud.com/developer-images/article/3335308/a1ig8opz1a.png?q-sign-algorithm=sha1&q-ak=AKID2uZ1FGBdx1pNgjE3KK4YliPpzyjLZvug&q-sign-time=1594733712;1594740912&q-key-time=1594733712;1594740912&q-header-list=&q-url-param-list=&q-signature=bd699ab1161ed5528bdff9db9c65dca14a40fa65)

![根目录](https://ask.qcloudimg.com/http-save/3335308/ka8xvx13jm.png)

![EXP文件夹](https://ask.qcloudimg.com/http-save/3335308/m9qm7mj9bt.png)

Exp，即为表情的动作，都是json文件，任意打开一个：F\_FUN\_SMILE.exp.json

通过格式化json文件，看得出这是控制每一个贴图材质的控件

![](https://ask.qcloudimg.com/http-save/3335308/s8p9hozana.png)

| Type | 类型 |
|:----|:----|
| Fade in/out | 动作淡入淡出 |
| Params：{ | ​ |
| id | 动作对象控件，例如眼睛，嘴，笑容等 |
| val | 动作幅度 |
| calc | 未知 |


----

MOC文件夹：

![](https://ask8088-private-1251520898.cn-south.myqcloud.com/developer-images/article/3335308/k97pgmlfms.png?q-sign-algorithm=sha1&q-ak=AKID2uZ1FGBdx1pNgjE3KK4YliPpzyjLZvug&q-sign-time=1594733867;1594741067&q-key-time=1594733867;1594741067&q-header-list=&q-url-param-list=&q-signature=7927b01a258e61dc72f966b493df2e9691799f9e)

这是一个存储模型和材质的文件，moc即为模型

上面的文件夹即为材质

![](https://ask.qcloudimg.com/http-save/3335308/2erriwckee.png)

材质中包括了人物的所有外饰信息

mtn文件夹

![](https://ask.qcloudimg.com/http-save/3335308/671wyfam2a.png)

![](https://ask.qcloudimg.com/http-save/3335308/042s7i2dc2.png)

这个是控制组件淡入淡出，角度，比如人体的x,y,z轴的旋转，以及防止动作突发诡异现象，使用淡入淡出

----

assets根目录的两个json文件：

![](https://ask.qcloudimg.com/http-save/3335308/osw6oxxwtc.png)

model.json即定义了模型，Physics.json即定义了物理效果model.json即定义了模型，Physics.json即定义了物理效果

![](https://ask.qcloudimg.com/http-save/3335308/kplefce3do.png)

![](https://ask.qcloudimg.com/http-save/3335308/ec650x5rq3.png)

这里相当于总控制器，控制了什么情况下对应什么动作的json，而动作的json定义了材质切片的运动，Physics物理效果就能更加真实地显示动画，比如头发摇摆

# Web中的呈现：

核心：live2d.js

![](https://ask.qcloudimg.com/http-save/3335308/5b59o5zy2w.png)

可惜是加密的

![](https://ask.qcloudimg.com/http-save/3335308/utryz2a17r.png)

我们无法进一步研究，但根据官方的文档来看，是运用webGL技术进行渲染的

首先要定义

```
<canvas id="live2d" width="280" height="250"></canvas>
```

然后引入js 

```
<script src="./js/live2d.js"></script>
```

最后加载js：

```
loadlive2d("live2d", "./model.json");
```

----

注意：json等模型是具有版权的，要不自己制作，要不就请求授权，尤其是应用于商业用途要及其注意

# 到此，介绍结束，更多可能需要大家一同探索

我是Ar-Sr-Na，个人网站www.arsrna.cn
