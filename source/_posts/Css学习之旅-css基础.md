## 1. Css使用的注释
>  /\*   */

## 2. 各个浏览器Css前缀
> **-webkit-** Chrome Safia<br>
> **-moz-** 火狐<br>
> **-ms-**<br>

## 3. Hack方式，处理不通浏览器之间的兼容问题参考：[Hack](http://blog.csdn.net/freshlover/article/details/12132801)

## 4. 样式表
外部
 
    <pre class="prettyprint">
    <link rel="stylesheet" href="../css/ym.css">
    </pre>

内部
 
    <pre class="prettyprint">
    <head>
    <style>
    body {
    	backgroud-color:#fff;
    }
    </style>
    </head>
    </pre>

内联

    <pre class="prettyprint">
    <div style="width:100;"></div>
    </pre>

## 5. div背景
>  background:color  image  repeat  attachment  position  
>  background-color:  #xxxx   rgb(22,22,22)
>  background-image : url()
>  覆盖background中图片背景background-image:none!important;
>  background-repeat: no-repeat/repeat
>  background-attachment: scroll/fixed/inherit（集成父亲）
>  background-position: 属性值

## 6. 文本
> 颜色：color:#xxx /rgb()
对齐方式：text-align:left/right/center/justify(两边对齐)
文字修饰：text-decoration:underline(下划线)/overline(上划线)/line-through(中间线)/none/inherit
文字转换：text-transform:uppercase/lowercase/capitalize（首字母大写）
文本缩进：text-indent: 33px;
字符间距离：letter-spacing：
单词之间的距离:word-spacing: 5px;
行间距：line-height: 10px;
文字方向：direction:rtl/ltr
文字阴影效果：text-shadow:-2px水平/ 2px垂直/ 1px 模糊的距离/ #FF0000阴影效果;
html段落文字中空白行处理:white-space:pre 空格回车都显示/normal空白被忽略/nowrap同一行显示直到遇到

## 7. 字体
> 字体样式：font-style: italic;
字体大小：font-size:1em和当前字体大小相等。在浏览器中默认的文字大小是16px 
解决浏览器字体大小问题body {font-size:100%;}
字体系列：font-family: "楷体";如果字体系列的名称超过一个字，它必须用引号，如Font Family："宋体"。
字体加粗：font-weight:bold/bolder更粗   /lighter:100-900  (bold 700)

## 8. 链接
> a:link - 正常，未访问过的链接
a:visited - 用户已访问过的链接
a:hover - 当用户鼠标放在链接上时
a:active - 链接被点击的那一刻

## 9. 列表
> 使用自定义方式设置列表项标记：
ul { list-style-type: none; padding: 0px; margin: 0px; } 
ul li { background-image: url(sqpurple.gif); background-repeat: no-repeat; background-position: 0px 5px; padding-left: 14px; }
常用列表项标记设置：list-style-type：[属性值](http://www.runoob.com/cssref/pr-list-style-type.html);

## 10. 表格
### **ps**:body上添加class设置css时候使用table.class-vale  来覆盖系统的table样式，不然有些样式显示不出来

>折叠边框：border-collapse:collapse(折叠边框使得table去掉外边框)/
![table](http://ovpqrf5pq.bkt.clouddn.com/css_table2.png)
>  左图是使用collapse ；有图默认的效果
表格描述:标签caption，设置caption-side: top/bottom/left/right;显示位置
表格样式：border-style: dotted dashed solid double;可以设置上下左右 border-style: top bottom left right;
![table](http://ovpqrf5pq.bkt.clouddn.com/css_table3.png)
>边框圆角：border-radius: 10px;可以设置上下左右 border-radius: top bottom left right;
边框颜色：border-color:
外边框：outline: green solid 9px;位于边框的外围

## 11. 盒子
>盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容
![table](http://ovpqrf5pq.bkt.clouddn.com/css_box.png)
如图：100 *100 就是我们设置div时候的width 和height的高度
在设计时候一定要注意并不包含padding border和margin的距离

## 12. 分组和嵌套
分组使用，减少相同样式的重复编写，不通的选择器使用逗号分隔

## 13. 显示
>  显示主要是display和visibility
visibility:hidden可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。
块和内联：display:block/inherit
块元素是一个元素，占用了全部宽度，在前后都是换行符。如下图h4元素和p元素
![table](http://ovpqrf5pq.bkt.clouddn.com/css_display1.png)
内联元素只需要必要的宽度，不强制换行。 如下将块元素h4和p的样式display设置为inline后
![table](http://ovpqrf5pq.bkt.clouddn.com/css_display2.png)

## 14. 定位
>Static定位，正常的位置
Fixed定位：固定的位置，不会随着滑动而移动位置
Relative定位：相对于该内容不加任何位置属性时候位置的偏移
Absolute定位：这个是位置相对最近的已定位的父布局，如果没有定位的父元素则相对于html
重叠的元素：z-index设置元素堆叠顺序，这个可以搭配Absolute使用，在设定了定位的父元素
中相对于其他的兄弟元素堆叠顺序
![table](http://ovpqrf5pq.bkt.clouddn.com/css_pos1.png)&nbsp;![table](http://ovpqrf5pq.bkt.clouddn.com/css_pos2.png)
左边使用了堆叠效果， 右边是未使用
剪切Absolute定位的元素：clip:rect(top, right, bottom, left);需要同时使用position:absolute;

#### 区域溢出：overflow:scroll/hidden/visible
> ![table](http://ovpqrf5pq.bkt.clouddn.com/css_overflow1.png)&nbsp;&nbsp;![table](http://ovpqrf5pq.bkt.clouddn.com/css_overflow2.png)&nbsp;&nbsp;![table](http://ovpqrf5pq.bkt.clouddn.com/css_overflow3.png)
上面图一：使用了hidden所有超出区域的部分都隐藏
图二：使用了scroll当超出了区域则使用滑动
图三：使用了visible当超出了仍然显示

## 15. 浮动
> float:left/right   相对的如果想取消浮动可以使用clean:both
一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止
![table](http://ovpqrf5pq.bkt.clouddn.com/css_float.png)
如上图，设置了img浮动，C位置p浮动，A位置P在不设置情况下，会紧靠着img,
如果img高度和C位置的P元素高度相同，那么A位置会排在B位置，
现在如果想A位置排在B位置就需要设置clean:both清除浮动效果

## [学习的Demo点击查看](https://coding.net/u/Leo--Liu/p/WebDemo/git)

