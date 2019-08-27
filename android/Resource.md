<script type="text/javascript">
    let title = document.querySelector("h1.project-name");
    title.innerText = "王侯不拜雨烟屐, 方知无欲是逍遥";
    title.style.fontStyle = 'italic';
</script>

# 应用资源


## 概述

### res目录

Android中每种资源都有自己指定的目录存放,如图所示

![资源文件目录](https://baiyuas.github.io/img/basic01.png)

res/目录下包含了所有的资源目录,各目录说明:

- anim  xml形式定义了补间动画

- animator xml形式定义的帧动画

- color 定义的颜色资源

- drawable 图片资源,支持.png,.9.png,.jpg,.gif格式的图片以及其他可绘制资源

- font 字体资源

- xml xml文件

- interpolator xml形式的动画插值器 

- layout 页面布局文件

- menu 导航菜单

- mipmap 主要存放启动图标

- navigation 导航布局

- values 样式文件,颜色,尺寸,字符串,数据等资源

- trasition 场景动画资源

- raw 二进制文件,例如pdf文件,office文件,音视频文件,zip文件等


### 文件夹名称限定

对于App的不同环境,Android提供了替代资源,例如对于不同分辨率的手机可以选择不同分辨率的图片,不同语言环境可以展示不同的语言的文字等. Android是如何实现的呢?

在Android中通过在res/目录下创建限定标识的资源目录,格式如:`<resources_name>-<config_qualifier>`

- resources_name 默认的资源目录

- config_qualifier 限定需求

这样配置后,Android系统会根据不同配置和环境来选取对应文件夹下的资源.

Android中支持的一些限定标识

| 限定类型        | 示例   |  说明  |
| :-----   | :-----  | :----  |
| 布局方向     | ldrtl <br>ldltr |  设置App布局方向 <br>**ldltr**是 "layout-direction-left-to-right" 缩写<br> **ldrtl**是 "layout-direction-right-to-left" 缩写 <br>该限定类型支持layout,drawable 或者values <br>开启该功能需要设置supportsRtl为"true",目标版本>= 17 |

| 语言       |  例如:<br>en<br>fr<br>en-rUS<br>fr-rFR<br>fr-rCA<br>b+en<br>b+en+US<br>b+es+419    |   对于支持的语言可以通过AndroidStudio创建目录(下面values部分会说明)   |

| 最小宽度        |    sw<N>dp<br>例如:<br>sw320dp<br>sw600dp<br>sw720dp等    |  的  |





### 应用资源匹配

文件选取

兼容

### 资源文件引用

## 资源类型

### anim

res/anim目录下定义了补间动画,

### color

### values

多语言支持
新建一个需要支持语言的目录 File->New->Android Resource Directory
![添加Local]()
![添加选择支持的语音]()


## 屏幕切换

字体https://developer.android.google.cn/guide/topics/ui/look-and-feel/fonts-in-xml?hl=en


参考
[Transition](https://www.jianshu.com/p/e497123652b5)
[Transition2](https://www.jianshu.com/p/1007f300f17a)