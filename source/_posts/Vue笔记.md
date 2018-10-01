## 常见指令

###  ***v-bind***

绑定属性，<span v-bind:title="title">
可以缩写:title

###  ***v-for***

循环指定
	
	<li v-for="todo in todos">
		{{todo.text}} 排行第{{todo.index}}
    </li>

###  ***v-if***

判断指定，与之相同的还有`v-show`，`v-show`是控制css中的`display`属性

###  ***v-on***

点击指定，使用Vue中的 `methods` 属性实现点击方法

### ***v-model***

双向绑定

## Vue中的属性

***el***

绑定的是标签的ID，

	new Vue({
		el: '#ddd'
	})

***data***
	
	var data = {a};
	var vm = new Vue({
		el: '#ddd',
		data: data
	})


当我们使用Vue中的属性时候，为了区分Vue原始的属性，使用 'vm.$el', 'vm.$data'
而data赋值的属性则可以直接使用`vm.a === data.a`

Vue中的属性都是响应式的，当data中的值发生变化时候，vm中的数据也会变化。如果想要
取消使用`Object.freeze()`。 例如`Object.freeze(data)`后，这会阻止修改现有的属性，
也意味着响应系统无法再追踪变化。

监控Vue属性变化可以使用`watch`属性，

	vm.$watch('a', function (newValue, oldValue) {
	  // 这个回调将在 `vm.a` 改变后调用
	})

## Vue的生命周期

<center>
<img src="https://cn.vuejs.org/images/lifecycle.png" alt="Vue生命周期" width="383px">
<br>
(图片来自<a href="https://cn.vuejs.org/v2/guide/instance.html#%E6%95%B0%E6%8D%AE%E4%B8%8E%E6%96%B9%E6%B3%95">Vue中文文档官方网站</a>)</center>



图中红色字体代表Vue的生命周期函数，在Vue创建到对应阶段会调用这些函数

* ***beforeCreate***
* ***created***
* ***beforeMount***
* ***mounted***
* ***beforeUpdate***
* ***updated***
* ***beforeDestroy***
* ***destroyed***

*注: 在Vue创建过程中，el创建时在`beforeMount`函数执行开始。如果Vue中没有设置el
则Vue的生命周期到`created`结束。更多生命周期解析可以参考[详解Vue生命周期](https://segmentfault.com/a/1190000011381906)*

## 模板语法

#### 插值

Vue中使用的是双大括号的方式插入文本`{{msg}}`，msg会随着绑定的值变化，如果不想变化使用`v-once`	指令

	<span v-once>这里不会变化 {{msg}} </span>

如果要插入html代码需要使用`v-html`，否则会显示原始代码。
	
	<p>This is Html Effect <span v-html="htmlData"></span></p>

对于Html中的属性值，需要使用`v-bind`来绑定

	<p v-bind:id="pId"></p>




