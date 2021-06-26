## jQuery核心函数

jQuery核心函数

 简称: jQuery函数($/jQuery)

 jQuery库向外直接暴露的就是$/jQuery

引入jQuery库后, 直接使用$即可

   当函数用: $(xxx)

   当对象用: $.xxx()

1.作为一般函数调用: $(param)

 	1). 参数为函数 : 当DOM加载完成后，执行此回调函数

```javascript
$(function(){    
})
```

 	2). 参数为选择器字符串: 查找所有匹配的标签, 并将它们封装成jQuery对象

```javascript
$('#btn')
```

 	3). 参数为DOM对象: 将dom对象封装成jQuery对象

```javascript
//this是什么? 发生事件的dom元素(<button>)
$('#btn').click(function () { // 绑定点击事件监听
    alert($(this).html())
    $('<input type="text" name="msg3"/><br/>').appendTo('div')
})
```

 	4). 参数为html标签字符串 (用得少): 创建标签对象并封装成jQuery对象

```javascript
$('<input type="text" name="msg3"/><br/>')
```

2.作为对象使用: $.xxx()

 	1). $.each() : 隐式遍历数组

```javascript
var arr = [2, 4, 7]
  // 1). $.each() : 隐式遍历数组
  $.each(arr, function (index, item) {
    console.log(index, item)
  })
```

 	2). $.trim() : 去除两端的空格

```javascript
var str = ' my atguigu  '
  // console.log('---'+str.trim()+'---')
console.log('---'+$.trim(str)+'---')
```

## jQuery核心对象

jQuery核心对象

 \* 简称: jQuery对象

 \* 得到jQuery对象: 执行jQuery函数返回的就是jQuery对象

 \* 使用jQuery对象: $obj.xxx()

1.jQuery对象是一个包含所有匹配的任意多个dom元素的伪数组对象

2.基本行为

 \* **size() / length**: 包含的DOM元素个数

```javascript
var $buttons = $('button')
  /*size()/length: 包含的DOM元素个数*/
console.log($buttons.size(), $buttons.length)
```

 \* **[index] / get(index)**: 得到对应位置的DOM元素

```javascript
$buttons[1].innerHTML, $buttons.get(1).innerHTML
```

 \* **each()**: 遍历包含的所有DOM元素

```javascript
$buttons.each(function (index, item) {
    //this对应的就是此时的item
    console.log(index, item.innerHTML, this)
})
$buttons.each(function () {
    console.log(this.innerHTML)
})
```

 \* **index()**: 得到在所在兄弟元素中的下标

```javascript
//获得btn3在伪数组中的下标
$('#btn3').index() //2
```

### 伪数组

 伪数组

  	\* 是个Object对象

  	\* 拥有length属性

  	\* 有数值下标属性

  	\* 没有数组特别的方法: forEach(), push(), pop(), splice()

## 选择器

### 基本选择器

 #id : id选择器

```javascript
//选择id为div1的元素
$('#div1').css('background', 'red')
```

 element : 元素选择器

```javascript
//选择所有的div元素
$('div').css('background', 'red')
```

 .class : 属性选择器

```javascript
// 选择所有class属性为box的元素
$('.box').css('background', 'red')
```

\* : 任意标签

```javascript
$('*').css('background', 'red')
```

 selector1,selector2,selectorN : 取多个选择器的并集(组合选择器)

```javascript
//选择所有的div和span元素
$('div,span').css('background', 'red')
```

 selector1selector2selectorN : 取多个选择器的交集(相交选择器)

```javascript
//选择所有class属性为box的div元素
$('div.box').css('background', 'red')
```

### 层次选择器

层次选择器: 查找后代元素, 子元素,  兄弟元素的选择器

ancestor descendant

 在给定的祖先元素下匹配所有的后代元素

```javascript
// 选中ul下所有的的span
$('ul span').css('background', 'yellow')
```

2.parent>child

 在给定的父元素下匹配所有的子元素

```javascript
// 选中ul下所有的子元素span
$('ul>span').css('background', 'yellow')
```

prev+next

 匹配所有**紧接**在 prev 元素后的 next 元素

```javascript
// 选中class为box的下一个li
$('.box+li').css('background', 'yellow')
```

4. prev~siblings

 匹配 prev 元素之后的所有 siblings 元素

```javascript
//选中ul下的class为box的元素后面的所有兄弟元素
$('ul .box~*').css('background', 'yellow')
```

### 过滤选择器

:first

```javascript
// 选择第一个div
$('div:first').css('background', 'red')
```

:last

```javascript
//选择最后一个class为box的元素
$('.box:last').css('background', 'red')
```

:not()

```javascript
// 选择所有class属性不为box的div
$('div:not(.box)').css('background', 'red')  
```

:gt(下标)大于    :lt(下标)小于

```javascript
// 选择第二个和第三个li元素
$('li:gt(0):lt(2)').css('background', 'red') // 多个过滤选择器不是同时执行, 而是依次
//$('li:lt(3):gt(0)').css('background', 'red')
```

:contains(元素里的内容)

```javascript
<li title="hello">BBBBB</li>
// 选择内容为BBBBB的li
$('li:contains("BBBBB")').css('background', 'red')
```

:hidden 选择隐藏的元素

```javascript
// 选择隐藏的li
console.log($('li:hidden').length, $('li:hidden')[0])
```

[title]   [title="属性值"]

```javascript
// 选择有title属性的li元素
$('li[title]').css('background', 'red')
//选择所有属性title为hello的li元素
$('li[title="hello"]').css('background', 'red')
```

### 表单选择器

:text  选择type为text的input

:checkbox   选择type为checkbox的input

:submit    选择type为submit的input

.........type为啥的都是差不多的

:disabled  选择不可用的，选择有disabled属性的

```javascript
// 选择不可用的文本输入框
$(':text:disabled').css('background', 'red')
```

:selected   选择有selected属性的

```javascript
<option value="2" selected="selected">天津</option>
$('select>option:selected').html()
```

***表单选择器中都是  :表单对象属性   来查找相应的属性***

## $工具方法

1.$.each(): 遍历数组或对象中的数据

```javascript
var obj = {
    name: 'Tom',
    setName: function (name) {
        this.name = name
    }
}
$.each(obj, function (key, value) {
    console.log(key, value)
  })
```

2.$.trim(): 去除字符串两边的空格

3.$.type(obj): 得到数据的类型

```javascript
console.log($.type($)) // 'function'
```

4.$.isArray(obj): 判断是否是数组

```javascript
console.log($.isArray($('body')), $.isArray([])) // false true
```

5.$.isFunction(obj): 判断是否是函数

```javascript
console.log($.isFunction($)) // true
```

6.$.parseJSON(json) : 解析json字符串转换为js对象/数组

```javascript
// json对象===>JS对象
var json = '{"name":"Tom", "age":12}'  // json对象: {}
console.log($.parseJSON(json))
// json数组===>JS数组
json = '[{"name":"Tom", "age":12}, {"name":"JACK", "age":13}]' // json数组: []
console.log($.parseJSON(json))
```

题外话

json字符串--->js对象/数组     JSON.parse(jsonString)

js对象/数组--->json字符串	 JSON.stringify(jsObj/jsArr)

## 属性、方法

1.操作任意属性

  	**attr()**  读取或修改属性值

```javascript
//读取第一个div的title属性
$('div:first').attr('title')
// 给所有的div设置name属性(value为atguigu)
$('div').attr('name', 'atguigu')
```

  	**removeAttr()**   移除属性

```javascript
// 移除所有div的title属性
$('div').removeAttr('title')
```

  	**prop()**   方法设置或返回被选元素的属性和值

​			- 当该方法用于**返回**属性值时，则返回第一个匹配元素的值。

​			- 当该方法用于**设置**属性值时，则为匹配元素集合设置一个或多个属性/值对。

```javascript
//返回属性的值：
$(selector).prop(property)
//设置属性和值：
$(selector).prop(property,value)
//设置多个属性和值：
$(selector).prop({property:value, property:value,...})
```

***attr(): 操作属性值为非布尔值的属性***

***prop(): 专门操作属性值为布尔值的属性***

2.操作class属性

  	**addClass()**   添加class属性

```javascript
//给所有的div添加class='abc'
$('div').addClass('abc')
```

 	 **removeClass()**  方法从被选元素移除一个或多个类

​		- 规定要移除的 class 的名称。

​		- 如需移除若干类，请使用空格来分隔类名。

​		- 如果不设置该参数，则会移除所有类。

```javascript
//移除所有div中class为guiguClass的class
$('div').removeClass('guiguClass')
```

 3.操作HTML代码/文本/值

  	**html()**  

```javascript
//得到最后一个li的标签体文本
console.log($('li:last').html())
```

  	**val()**   大部分是获取输入框中的value值

```javascript
//得到输入框中的value值
console.log($(':text').val()) // 读取
//将输入框的值设置为atguigu
$(':text').val('atguigu') // 设置      读写合一
```

## CSS

### 读取/设置CSS

读取CSS样式

```javascript
//得到第一个p标签的颜色
$('p:first').css('color')
```

设置CSS样式

```javascript
//设置所有p标签的文本颜色为red
$('p').css('color', 'red')
//设置第2个p的字体颜色(#ff0011),背景(blue),宽(300px), 高(30px)
$('p:eq(1)').css({
    color: '#ff0011',
    background: 'blue',
    width: 300,
    height: 30
})
```

### 位置

#### 获取/设置标签的位置数据

 \* offset(): 相对页面左上角的坐标

 \* position(): 相对于父元素左上角的坐标

#### 获取/设置滚动条的位置数据

scrollTop():   读取/设置滚动条的Y坐标

scrollLeft():	读取/设置滚动条的X坐标

```javascript
//读取页面滚动条的Y坐标(兼容chrome和IE)
$(document.body).scrollTop()+$(document.documentElement).scrollTop()
$('html').scrollTop()+$('body').scrollTop()
//滚动到指定位置(兼容chrome和IE) 
$('body,html').scrollTop(60);  
```

### 尺寸

 1.内容尺寸

​	 height(): height

 	width(): width

2.内部尺寸

​	 innerHeight(): height+padding

 	innerWidth(): width+padding

3.外部尺寸（默认false）

 	outerHeight(false/true): height+padding+border 如果是true, 加上margin

 	outerWidth(false/true): width+padding+border 如果是true, 加上margin

```javascript
var $div = $('div')
// 1. 内容尺寸
console.log($div.width(), $div.height())  // 100 150
// 2. 内部尺寸
console.log($div.innerWidth(), $div.innerHeight()) //120 170
// 3. 外部尺寸
console.log($div.outerWidth(), $div.outerHeight()) //140 190
console.log($div.outerWidth(true), $div.outerHeight(true)) //160 210
```

## 筛选

### 过滤

在jQuery对象中的元素对象数组中过滤出一部分元素来

**first()**   找出该元素对象数组中的第一个元素

```javascript
$('li').first().css('background', 'red')
```

**last()**	找出该元素对象数组中的最后一个元素

```javascript
$('li').last().css('background', 'red')
```

**eq(index|-index)**	找出该元素对象数组中的第index个元素(下标从0开始)

```javascript
//li标签的第二个
$('li').eq(1).css('background', 'red')
```

**filter(selector)**	过滤出符合条件的元素

```javascript
//li标签中title属性为hello的
$('li').filter('[title=hello]').css('background', 'red')
```

**not(selector)**		过滤出不含条件的元素

```javascript
// li标签中title属性不为hello的
$('li').not('[title=hello]').css('background', 'red')
```

**has(selector)**		保留包含特定后代的元素，去掉那些不含有指定后代的元素。

```javascript
//li标签中有span子标签的
$('li').has('span').css('background', 'red')
```

### 查找

在已经匹配出的元素集合中根据选择器查找孩子/父母/兄弟标签

1.children(): 子标签中找

```javascript
//ul标签的第2个span子标签
$('ul').children('span:eq(1)').css('background', 'red')
```

2.find() : 后代标签中找

```javascript
// ul标签的第2个span后代标签
$ul.find('span:eq(1)').css('background', 'red')
```

3.parent() : 父标签

```javascript
// ul标签的父标签
$ul.parent().css('background', 'red')
```

4.prevAll() : 前面所有的兄弟标签

```javascript
// id为cc的li标签的前面的所有li标签
$('#cc').prevAll('li').css('background', 'red')
```

5.nextAll() : 后面所有的兄弟标签

```javascript
// id为cc的li标签的后面的所有li标签
.nextAll('li').css('background', 'red')
```

6.siblings() : 前后所有的兄弟标签

```javascript
//id为cc的li标签的所有兄弟li标签
$('#cc').siblings('li').css('background', 'red')
```

## 文档处理

1.添加/替换元素

 \* **append(content)**

  向当前匹配的所有元素内部的最后插入指定内容

**appendTo**(父元素)  将当前匹配的所有元素的插入到指定元素里

```javascript
//向id为ul1的ul下添加一个span(最后)
$('<span>appendTo()添加的span</span>').appendTo($('#ul1'))
```

 \* prepend(content)

  向当前匹配的所有元素内部的最前面插入指定内容

 \* before(content)

  将指定内容插入到当前所有匹配元素的前面

 \* after(content)

  将指定内容插入到当前所有匹配元素的后面替换节点

 \* replaceWith(content)

  用指定内容替换所有匹配的标签删除节点

2.删除元素

 \* empty()

  删除所有匹配元素的子元素

 \* remove()

  删除所有匹配的元素

## 事件

事件绑定(2种)：

 \* eventName(function(){})

  绑定对应事件名的监听, 例如：$('#div').click(function(){});

 \* on(eventName, funcion(){})

  通用的绑定事件监听, 例如：$('#div').on('click', function(){})

 \* 优缺点:

  eventName: 编码方便, 但只能加一个监听, 且有的事件监听不支持

  on: 编码不方便, 可以添加多个监听, 且更通用

2.事件解绑：

 \* off(eventName)

事件的坐标

 \* event.clientX, event.clientY 相对于视口的左上角

 \* event.pageX, event.pageY 相对于页面的左上角

 \* event.offsetX, event.offsetY 相对于事件元素左上角

事件相关处理

 \* 停止事件冒泡 : event.stopPropagation()

 \* 阻止事件默认行为 : event.preventDefault()

### 区别mouseover与mouseenter

**mouseover**: 在移入子元素时也会触发, 对应mouseout

**mouseenter**: 只在移入当前元素时才触发, 对应mouseleave

​                hover()使用的就是mouseenter()和mouseleave()

```javascript
$('.div1')
    .mouseover(function () {
    console.log('mouseover 进入')
})
    .mouseout(function () {
    console.log('mouseout 离开')
})

$('.div3')
    .mouseenter(function () {
    console.log('mouseenter 进入')
})
    .mouseleave(function () {
    console.log('mouseleave 离开')
})
```

### 事件委托

事件委托(委派/代理):	(原生JS)

 \* 将多个子元素(li)的事件监听委托给父辈元素(ul)处理

 \* 监听回调是加在了父辈元素上

 \* 当操作任何一个子元素(li)时, 事件会冒泡到父辈元素(ul)

 \* 父辈元素不会直接处理事件, 而是根据event.target得到发生事件的子元素(li), 通过这个子元素调用事件回调函数

jQuery的事件委托API

**设置事件委托**: $(parentSelector).delegate(childrenSelector, eventName, callback)

**移除事件委托**: $(parentSelector).undelegate(eventName)

```javascript
// 设置事件委托
$('ul').delegate('li', 'click', function () {
    // console.log(this)
    this.style.background = 'red'
})
// 移除事件委托
$('ul').undelegate('click')
```

## 效果

### 显示与隐藏

显示隐藏，默认没有动画, 动画(opacity/height/width)

1.show(): (不)带动画的显示

```javascript
//立即显示
$('#btn1').click(function () {
    $('.div1').show()
})
//慢慢显示
$('#btn1').click(function () {
    $('.div1').show(1000)
})
```

2.hide(): (不)带动画的隐藏

```javascript
//立即隐藏
$('#btn1').click(function () {
    $('.div1').hide()
})
//慢慢隐藏
$('#btn1').click(function () {
    $('.div1').hide(1000)
})
```

3.toggle(): (不)带动画的切换显示/隐藏

```javascript
//切换显示/隐藏
$('#btn4').click(function () {
    $('.div1').toggle(1000)
})
```

### 自定义动画

jQuery动画本质 : 在指定时间内不断改变元素样式值来实现的

1.animate(): 自定义动画效果的动画

```javascript
$('#btn1').click(function () {
    /*
    $('.div1').animate({
      width: 200,
      height: 200
    }, 1000)
    */
    $('.div1')
        .animate({
        width: 200
    }, 1000)
        .animate({
        height: 200
    }, 1000)
})
```

2.stop(): 停止动画

```javascript
$('#btn4').click(function () {
	$('.div1').stop()
})
```

## 多库共存

如果引进来的库也要使用 $，为了能够正常使用，jQuery库可以释放$的使用权

```javascript
// 释放$的使用权
jQuery.noConflict()
// 要想使用jQuery的功能, 只能使用jQuery
jQuery(function () {
    console.log('文档加载完成')
})
```

## onload和ready

区别: window.onload与 $(document).ready()

 \* window.onload

​	- 包括页面的图片加载完后才会回调(晚)

​	- 只能有一个监听回调

 \* $(document).ready()

​	- 等同于: $(function(){})

​	- 页面加载完就回调(早)

​	- 可以有多个监听回调

早----->晚

$(function(){}) < $(document).on('load', function(){}) < windows.onload = function(){}

## 自定义插件

1.扩展jQuery的工具方法

 $.extend(object)

2.扩展jQuery对象的方法

 $.fn.extend(object)

先新建一个js文件，并将新建的js文件引入你的文档中，在新建的js文件中添加你自定义的工具方法或对象方法

```javascript
//js文件
(function () {
  // 扩展$的方法
  $.extend({
    min: function (a, b) {
      return a < b ? a : b
    },
    max: function (a, b) {
      return a > b ? a : b
    },
    leftTrim: function (str) {
      return str.replace(/^\s+/, '')
    },
    rightTrim: function (str) {
      return str.replace(/\s+$/, '')
    }
  })
  // 扩展jQuery对象的方法
  $.fn.extend({
    checkAll: function () {
      this.prop('checked', true) // this是jQuery对象
    },
    unCheckAll: function () {
      this.prop('checked', false)
    },
    reverseCheck: function () {
      // this是jQuery对象
      this.each(function () {
        // this是dom元素
        this.checked = !this.checked
      })
    }
  })

})()
```

在文档中使用自定义的方法

```javascript
//使用自定义的$方法
/*min(a, b) : 返回较小的值
max(c, d) : 返回较大的值
leftTrim() : 去掉字符串左边的空格
rightTrim() : 去掉字符串右边的空格*/
$.min(3, 5), $.max(3, 5)
var string = '   my atguigu    '
$.leftTrim(string),$.rightTrim(string)
//使用自定义的对象方法
/*checkAll() : 全选
unCheckAll() : 全不选
reverseCheck() : 全反选*/
var $items = $(':checkbox[name=items]')
$('#checkedAllBtn').click(function () {
    $items.checkAll()
})
$('#checkedNoBtn').click(function () {
    $items.unCheckAll()
})
$('#reverseCheckedBtn').click(function () {
    $items.reverseCheck()
})
```

