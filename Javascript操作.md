# DOM

## DOM查询

### childNodes和children属性

**childNodes**属性会获取包括文本节点在内的所有节点

\* 根据DOM标签标签间空白也会当成文本节点

\* 注意：在IE8及以下的浏览器中，不会将空白文本当成子节点

**children**属性可以获取当前元素的所有子元素

**children 属性与 childNodes 属性的差别**：

- childNodes 属性返回所有的节点，包括文本节点、注释节点；
- children 属性只返回元素节点；

**firstChild**可以获取到当前元素的第一个子节点（包括空白文本节点）

**firstElementChild**获取当前元素的第一个子元素，但是firstElementChild不支持IE8及以下的浏览器，如果需要兼容他们尽量不要使用

### 获取元素节点

**document.body** 获取body

**document.documentElement** 保存的是html根标签

**document.all**代表页面中所有的元素

**document.getElementById()**

可以根据id属性值获取一组元素节点对象，是动态获取，每次使用的时候都会重新获取元素

**document.getElementsByTagName()**

获取所有具有指定标签名的文档元素。是动态获取，每次使用的时候都会重新获取元素

**getElementsByName()** 

可以根据name属性获取一组元素节点对象的集合。是动态获取，每次使用的时候都会重新获取元素

**getElementsByClassName()**

可以根据class属性值获取一组元素节点对象， 但是该方法不支持IE8及以下的浏览器。是动态获取，每次使用的时候都会重新获取元素

**document.querySelector()**

- 需要一个选择器的字符串作为参数，可以根据一个CSS选择器来查询一个元素节点对象
- 虽然IE8中没有getElementsByClassName()但是可以使用querySelector()代替
- 使用该方法总会返回唯一的一个元素，如果满足条件的元素有多个，那么它只会返回第一个
- 是静态获取，只获取一次

```javascript
document.querySelector(".box1")
```

**document.querySelectorAll()**

- 该方法和querySelector()用法类似，不同的是它会将符合条件的元素封装到一个数组中返回
- 即使符合条件的元素只有一个，它也会返回数组
- 是静态获取，只获取一次

```javascript
document.querySelectorAll(".box1")
```

### 读取元素节点属性

如果需要读取元素节点属性，

\* 直接使用 **元素.属性名**

\*   例子：元素.id 元素.name 元素.value

\*   **注意**：class属性不能采用这种方式，读取class属性时需要使用 **元素.className**

**getAttribute("属性名")**  可以获取相应的属性值

```javascript
box.getAttribute("title")
```

**setAttribute("属性名","属性值")**    

​	- 添加指定的属性，并为其赋指定的值。

​	- 如果这个指定的属性已存在，则仅设置/更改值。

```javascript
box.setAttribute("title","dood")
```

**removeAttribute("属性名")**   移除相应的属性

```js
box.removeAttribute("title")
```

**innerHTML**用于获取元素内部的HTML代码的

\* 对于自结束标签，这个属性没有意义，要想获取自结束标签文本框中的值可以采用**元素.value**

\-使用innerHTML也可以完成DOM的增删改的相关操作

```javascript
city.innerHTML += "<li>广州</li>"
```

在id为city的ul DOM元素中添加一个li标签

**innerText**

- 该属性可以获取到元素内部的文本内容

- 它和innerHTML类似，不同的是它会自动将html去除

**previousSibling**

\-获取元素前一个兄弟节点（也可能获取到空白的文本）元素.previousSibling

**nextSibling**

获取元素之后紧跟的节点（也可能获取到空白的文本）元素.nextSibling

**previousElementSibling**获取前一个兄弟元素，IE8及以下不支持

**parentNode ** 获取元素的父节点

**childNodes**    获取元素所有的子节点，可能含有空白节点，返回的是一个数组

**firstChild**    获取元素的第一个子节点，与 childNodes[0] 完全等价

**lastChild**     获取元素的最后一个子节点

**nodeType**   可以让我们知道自己正在和哪一种属性打交道

返回的属性值	

​	\- 1   元素节点

​	\- 2   属性节点

​	\- 3   文本节点

**nodeValue**  获取的是节点的该属性值，大部分的元素节点的nodeValue属性为空，一般是用于获取文本节点的内容

```javascript
alert(document.getElementById("description").nodeValue)  //null
document.getElementById("description").childNodes[0].nodeValue
```

**nodeName**   获取的是元素的**标签名**，而且返回的是一个大写字母的值，即使元素在HTML文档里是小写字母。

**className**  添加/修改元素class属性

```js
box.className = "aa";
box.className += "bb" 
```

在html5 标注下 DOM 提供了一个API classList 操作calss名字

```js
box.classList.add("bb");
```

移除class属性值

```js
box.classList.remove("aa");
```

### 创建和添加节点

**document.createElement()** 可以用于创建一个元素节点对象

 \- 它需要一个标签名作为参数，将会根据该标签名创建元素节点对象，并将创建好的对象作为返回值返回

```javascript
document.createElement("li")
```

**document.createTextNode()** 可以用来创建一个文本节点对象

\- 需要一个文本内容作为参数，将会根据该内容创建文本节点，并将新的节点返回

```javascript
document.createTextNode("广州")
```

**appendChild()**

\-向一个父节点中添加一个新的子节点

\-用法：父节点.appendChild(子节点);

**insertBefore()**

\-可以在指定的子节点前插入新的子节点，也可以把本来在父节点的子节点移入到其他节点的前面

-语法：父节点.insertBefore(新节点,旧节点);

**replaceChild()**

\-可以使用指定的子节点替换已有的子节点

\-语法：父节点.replaceChild(新节点,旧节点);

**removeChild()**

\-可以删除一个子节点

\- 语法：父节点.removeChild(子节点);

 子节点.parentNode.removeChild(子节点);

### 弹出消息提示框

confirm()用于弹出一个带有确认和取消按钮的提示框

\* 需要一个字符串作为参数，该字符串将会作为提示文字显示出来

\* 如果用户点击确认则会返回true，如果点击取消则返回false

一般使用这个提示框的都是一个超链接，但点击了超链接，系统会默认跳转页面，为了消除默认行为都需要在最后return false

```javascript
var flag = confirm("确认删除"+name+"吗?");
if(flag){
//删除tr
	tr.parentNode.removeChild(tr);
}
//消除默认行为
return false;
```

## DOM操作CSS

### 通过JS修改元素的样式

\* 语法：**元素.style.样式名 = 样式值**

如果CSS的样式名中含有 - ，

\*   这种名称在JS中是不合法的比如background-color

\*   需要将这种样式名修改为驼峰命名法，

\*   去掉 - ，然后将 - 后的字母大写

我们通过style属性设置的样式都是内联样式

***通过style属性设置和读取的都是内联样式***

\* 无法读取样式表中的样式

```javascript
box1.style.width = "300px"
box1.style.height = "300px"
box1.style.backgroundColor = "yellow"
```

**批量修改css样式**

将多次改变样式属性的操作合并为一次操作（适用于单个存在的节点），减少页面重排reflow。

```js
box1.style.cssText = "color: red; text-align: left; font-size: 50px; text-decoration: overline; background-color: black;";
```

### currentStyle和getComputedStyle()获取元素样式

**currentStyle**获取元素的当前显示的样式

\* 语法：元素.currentStyle.样式名

\* 它可以用来读取当前元素正在显示的样式，如果当前元素没有设置该样式，则获取它的默认值

\*  currentStyle只有IE浏览器支持，其他的浏览器都不支持

```javascript
box1.currentStyle.width
box1.currentStyle["width"]
```

在其他浏览器中可以使用：

**getComputedStyle()**这个方法来获取元素当前的样式

\*   这个方法是window的方法，可以直接使用，但是该方法不支持IE8及以下的浏览器

\* 需要两个参数

​           \*   第一个：要获取样式的元素

​           \*   第二个：可以传递一个伪元素，一般都传null

```javascript
getComputedStyle(box1 , null).width
getComputedStyle(box1 , null)["width"]
```

***通过currentStyle和getComputedStyle()读取到的样式都是只读的， 不能修改，如果要修改必须通过style属性***

### 获取宽度高度、偏移量

**innerWidth  innerHeight**

-这两个属性可以获取可视区的宽高,包含滚动条的宽高,不包含控制台

**clientWidth   clientHeight**

\-这两个属性可以获取元素的可见宽度和高度 ( 包括内容区和内边距 )

**offsetWidth  offsetHeight**

\-获取元素的整个的宽度和高度，包括内容区、内边距和边框

**offsetParent**

\-可以用来获取当前元素的定位父元素

\-会获取到离当前元素最近的开启了定位的祖先元素

\-如果所有的祖先元素都没有开启定位，则返回body

**offsetLeft**

\-当前元素相对于其定位父元素的水平偏移量

**offsetTop**

\-当前元素相对于其定位父元素的垂直偏移量，元素的上外边框到包含元素的上内边框之间的像素

**scrollWidth  scrollHeight**

\-可以获取元素整个滚动区域的宽度和高度，不包含滚动条的宽

**scrollLeft**

\-可以获取水平滚动条滚动的距离

**scrollTop**

\-可以获取垂直滚动条滚动的距离

```javascript
var box1 = document.getElementById("box1");
box1.clientWidth
box1.clientHeight
/*其他的都是一样*/
```

***这些属性都是不带px的，返回都是一个数字，可以直接进行计算***

***这些属性都是只读的，不能修改***

## 音视频相关属性、事件

### 属性

currentTime：返回当前播放的位置，以秒表示

duration：返回媒体的总时长，以秒表示，对于流媒体返回无穷大

paused，表示媒体是否处于暂停状态

### 事件

play，在媒体播放开始时发生

pause，在媒体暂停时发生

loadeddata，在媒体可以从当前播放位置开始播放时发生

ended，在媒体已播放完成而停止时发生

## 表单属性

autocomplete，用于为文本（text）输入框添加一组建议的输入项

autofocus，用于让表单元素自动获得焦点

form，用于对<form>标签外部的表单元素分组

min、max和step，用在范围（range）和数值（number）输入框中

pattern，用于定义一个正则表达式，以便验证输入的值

placeholder，用于在文本输入框中显示临时性的提示信息

required，表示必填

## 事件

### 事件对象

事件对象 (**event**)

\-当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数,在事件对象中封装了当前事件相关的一切信息，比如：鼠标的坐标 键盘哪个按键被按下 鼠标滚轮滚动的方向。。。

**clientX** 可以获取鼠标指针的水平坐标

**cilentY** 可以获取鼠标指针的垂直坐标

**pageX**和**pageY**可以获取鼠标相对于当前页面的坐标

\- 但是这个两个属性在IE8中不支持，所以如果需要兼容IE8，则不要使用

```javascript
onmousemove
- 该事件将会在鼠标在元素中移动时被触发
areaDiv.onmousemove = function(event){
    //解决事件对象的兼容性问题
    event = event || window.event;

    var x = event.clientX;
    var y = event.clientY;

    //alert("x = "+x + " , y = "+y);
    showMsg.innerHTML = "x = "+x + " , y = "+y;			
};
```

**offsetX  offsetY**

-获取到是触发点相对被触发主体的左上角距离，以内容区左上角为基准点（不包括边框），如果触发点在边框上会返回负值

### 事件冒泡

事件的冒泡（Bubble）

\-所谓的冒泡指的就是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发

\-在开发中大部分情况冒泡都是有用的,如果不希望发生事件冒泡可以通过事件对象来取消冒泡。

**取消冒泡**：可以将事件对象的cancelBubble设置为true，即可取消冒泡

```javascript
event.cancelBubble = true;
```

**阻止冒泡：**事件到此为止,不会向上传递

```js
event.stopPropagation();
```

### 事件委派

事件的委派

\-指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素， 从而通过祖先元素的响应函数来处理事件。

\-事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能

target

\- event中的target表示的触发事件的对象

```javascript
u1.onclick = function(event){
    event = event || window.event;

    /*
		* target
		* 	- event中的target表示的触发事件的对象
		ie的target是event.srcElement
		var target = event.target || event.srcElement
	*/
    //alert(event.target);
    //如果触发事件的对象是我们期望的元素，则执行否则不执行
    if(event.target.className == "link"){
        event.target.style.color = "yellow";
        alert("我是ul的单击响应函数");
    }

};
```

### 事件绑定

使用 对象.事件 = 函数 的形式绑定响应函数，

\* 它只能同时为一个元素的一个事件绑定一个响应函数，

\* 不能绑定多个，如果绑定了多个，则后边会覆盖掉前边的

**addEventListener()**

\-通过这个方法也可以为元素绑定响应函数

\-参数：

​	1.事件的字符串，不要on 如：要写onclick 就写click

​	2.回调函数，当事件触发时该函数会被调用

​	3.是否在捕获阶段触发事件，需要一个布尔值，一般都传false

使用addEventListener()可以同时为一个元素的相同事件同时绑定多个响应函数，

​         \* 这样当事件被触发时，响应函数将会按照函数的绑定顺序执行

​         \* 这个方法不支持IE8及以下的浏览器

```javascript
btn01.addEventListener("click",function(){
    alert(1);
},false);
```

使用addEventListener()方法绑定响应函数，取消默认行为时不能使用return false

​           \* 需要使用event来取消默认行为  event.preventDefault();

​           \* 但是IE8不支持event.preventDefault();这个玩意，如果直接调用会报错

```javascript
event.preventDefault && event.preventDefault();
```



**attachEvent()**

\- 在IE8中可以使用attachEvent()来绑定事件

\- 参数：

​	1.事件的字符串，要on

​	2.回调函数

 - 这个方法也可以同时为一个事件绑定多个处理函数，不同的是它是后绑定先执行，执行顺序和addEventListener()相反

```javascript
btn01.attachEvent("onclick",function(){
    alert(1);
});
```

addEventListener()中的this，是绑定事件的对象

attachEvent()中的this，是window

​      需要统一两个方法this

```javascript
/*
obj 要绑定事件的对象
eventStr 事件的字符串(不要on)
callback 回调函数
*/
function bind(obj , eventStr , callback){
    if(obj.addEventListener){
        //大部分浏览器兼容的方式
        obj.addEventListener(eventStr , callback , false);
    }else{
        /*
			* this是谁由调用方式决定
			* callback.call(obj)
		*/
        //IE8及以下
        obj.attachEvent("on"+eventStr , function(){
            //在匿名函数中调用回调函数
            callback.call(obj);
        });
    }
}
```

### 事件传播

1.捕获阶段

​	- 在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件

2.目标阶段

​	- 事件捕获到目标元素，捕获结束开始在目标元素上触发事件

3.冒泡阶段

​	- 事件从目标元素向他的祖先元素传递，依次触发祖先元素上的事件

​	- 如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true

​         \*     一般情况下我们不会希望在捕获阶段触发事件，所以这个参数一般都是false

- ***IE8及以下的浏览器中没有捕获阶段***

### 其他事件

**setCapture()   releaseCapture()**

setCapture()捕获所有鼠标按下的事件

​           \* - 只有IE支持，但是在火狐中调用时不会报错，

​           \*   而如果使用chrome调用，会报错

```javascript
box1.setCapture && box1.setCapture();
//当鼠标松开时，取消对事件的捕获
box1.releaseCapture && box1.releaseCapture();
```

#### 鼠标滚轮事件

**onmousewheel**鼠标滚轮滚动的事件，会在滚轮滚动时触发，

\* 但是火狐不支持该属性

\* 在火狐中需要使用 **DOMMouseScroll** 来绑定滚动事件

​	\-  注意该事件需要通过addEventListener()函数来绑定

event.wheelDelta 可以获取鼠标滚轮滚动的方向

​	- wheelDelta这个值我们不看大小，只看正负

​	  wheelDelta这个属性火狐中不支持

\- 在火狐中使用event.detail来获取滚动的方向，***并且他们的方向相反***

#### 键盘事件

onkeydown

​	\- 按键被按下

​	\- 对于onkeydown来说如果一直按着某个按键不松手，则事件会一直触发

​	\- 当onkeydown连续触发时，第一次和第二次之间会间隔稍微长一点，其他的会非常的快，这种设计是为了防止误操作的发生。

onkeyup

​	\- 按键被松开

键盘事件一般都会绑定给一些可以**获取到焦点的对象**(如input)或者是**document**

属性：

altKey / ctrlKey / shiftKey

​           \*   - 这个三个用来判断alt ctrl 和 shift是否被按下

​           \*     如果按下则返回true，否则返回false

可以通过keyCode来获取按键的编码(ASCII值)

​           \* 通过它可以判断哪个按键被按下

```javascript
//判断y和ctrl是否同时被按下
if(event.keyCode === 89 && event.ctrlKey){
	console.log("ctrl和y都被按下了");
}
```

# BOM

BOM

 	-浏览器对象模型

 	\-BOM可以使我们通过JS来操作浏览器

​	 \-在BOM中为我们提供了一组对象，用来完成对浏览器的操作

BOM对象

Window

​          - 代表的是整个浏览器的窗口，同时window也是网页中的全局对象

Navigator

​          - 代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器

Location

​          - 代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息，或者操作浏览器跳转页面

History

​         - 代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录，由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页，而且该操作只在当次访问时有效

Screen

​          - 代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关的信息

这些BOM对象在浏览器中都是作为window对象的属性保存的，可以通过window对象来使用，也可以直接使用。

## Navigator

​        - 代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器

​        - 由于历史原因，Navigator对象中的大部分属性都已经不能帮助我们识别浏览器了

​        - 一般我们只会使用userAgent属性来判断浏览器的信息，

userAgent是一个字符串，这个字符串中包含有用来描述浏览器信息的内容，不同的浏览器会有不同的userAgent

如果通过UserAgent不能判断，还可以通过一些浏览器中特有的对象，来判断浏览器的信息

比如：ActiveXObject，判断IE11浏览器

## History

​	\-对象可以用来操作浏览器向前或向后翻页

属性：

​	length   可以获取到当成访问的链接数量

​	back()	可以用来回退到上一个页面，作用和浏览器的回退按钮一样

​	forward()	可以跳转下一个页面，作用和浏览器的前进按钮一样

​	go()

​	- 可以用来跳转到指定的页面

​            - 它需要一个整数作为参数

​           	1:表示向前跳转一个页面 相当于forward()

​               2:表示向前跳转两个页面

​              -1:表示向后跳转一个页面

​              -2:表示向后跳转两个页面

```javascript
alert(history.length);
history.back();
history.forward();
history.go(-2);
```

## Location

​	\- 该对象中封装了浏览器的地址栏的信息

如果直接打印location，则可以获取到地址栏的信息（当前页面的完整路径）

```javascript
alert(location);
```

如果直接将location属性修改为一个完整的路径，或相对路径，则我们页面会自动跳转到该路径，并且会生成相应的历史记录

```javascript
location = "http://www.baidu.com";
```

assign()

​       - 用来跳转到其他的页面，作用和直接修改location一样

```javascript
location.assign("http://www.baidu.com");
```

reload()

​           - 用于重新加载当前页面，作用和刷新按钮一样

​           - 如果在方法中传递一个true，作为参数，则会强制清空缓存刷新页面

```javascript
location.reload();
location.reload(true);
```

replace()

​           - 可以使用一个新的页面替换当前页面，调用完毕也会跳转页面，不会生成历史记录，不能使用回退按钮回退

```javascript
location.replace("01.BOM.html");
```

## 定时器

### 定时调用

**开启定时器**

let 变量 = setInterval(函数, 时间)

setInterval()      定时调用

​          - 可以将一个函数，每隔一段时间执行一次

\- 参数：

​           1.回调函数，该函数会每隔一段时间被调用一次

​           2.每次调用间隔的时间，单位是毫秒

 - 返回值：

​         \*   返回一个Number类型的数据

​         \*   这个数字用来作为定时器的唯一标识

**关闭定时器**   clearInterval(变量);

​	- 方法中需要一个定时器的标识作为参数，这样将关闭标识对应的定时器

clearInterval()可以接收任意参数，

​           \* 如果参数是一个有效的定时器的标识，则停止对应的定时器

​           \* 如果参数不是一个有效的标识，则什么也不做

### 延时调用

**开启延时调用**

let 变量= setTimeout(函数, 时间);

**关闭延时调用**

clearTimeout(变量);

延时调用一个函数不马上执行，而是隔一段时间以后在执行，而且只会执行一次

​	延时调用和定时调用的区别，定时调用会执行多次，而延时调用只会执行一次

 延时调用和定时调用实际上是可以互相代替的，在开发中可以根据自己需要去选择

# Ajax

核心就是XMLHttpRequest对象

但在IE中是使用ActiveXObject对象的，而且不同的IE版本，使用的XMLHTTP对象不同

```javascript
function getHTTPObject(){
    if(typeof XMLHttpRequest == "undefined"){
        XMLHttpRequest = function(){
            try { 
                return new ActiveXObject("Msxml2.XMLHTTP.6.0");
            } catch (e) {}
            try {
                return new ActiveXObject("Msxml2.XMLHTTP.3.0");
            } catch (e) {}
            try {
                return new ActiveXObject("Msxml2.XMLHTTP");
            } catch (e) {}
            return false;
        }
    }
    return new XMLHttpRequest();
}

var request = getHTTPObject();
```

XMLHttpRequest对象的readyState属性有五个值，表示状态值

​	- 0 表示未初始化

​	- 1 表示正在加载中

​	- 2 表示加载完毕

​	- 3 表示正在交互

​	- 4 表示完成

访问服务器发送回来的数据要通过两个属性来完成，responseText属性和responseXML属性。

responseText：用于保存文本字符串形式的数据。

responseXML：用于保存Content-Type头部中指定为"text/xml"的数据，其实是一个DocumentFragment对象，可以使用各种DOM方法来处理这个对象。

## get方法

```javascript
function sendMsg(){
    //1.创建一个XMLHttpRequest对象
    var xhr = new XMLHttpRequest();
    //2.调用open方法打开连接
    //open方法有三个参数：
    //  1.请求的method
    //  2.请求的url
    //  3.是否异步，默认值为true，一般这个参数可以不传
    xhr.open('get','http://localhost:3000/news?id=2')
    // 3.发送请求
    xhr.send()
    // 4.监听状态的改变
    xhr.onreadystatechange = function (){
        //判断状态值，0-4 五种状态 ，4代表最终的完成
        if(xhr.readyState === 4){
            //判断状态码
            if(xhr.status === 200){
                console.log(xhr.responseText)
                var resp = JSON.parse(xhr.responseText)
                console.log(resp)
                document.querySelector('div').innerHTML =`
                    <h2>编号:${resp.id}</h2>
                    <h2>标题：${resp.title}</h2>
                    `
            }
        }
    }
}
```

## post方法

post 比 get 多增加了一个setRequestHeader方法用来设置请求头的，post 的 open方法的 url 中不包含参数，将  url 中要传递的参数放到 send方法中去，url 只传递地址

```javascript
function sendMsg(){
    var xhr = new XMLHttpRequest()
    //这里method传post
    xhr.open('POST', 'http://localhost:3000/news')
    //设置请求头的content-type
    xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded")
    xhr.send("id=2")
    xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 ){
            if(xhr.status === 200){
                console.log(xhr.responseText)
                var resp = JSON.parse(xhr.responseText)
                console.log(resp)
                document.querySelector('div').innerHTML =`
                    <h2>编号:${resp.id}</h2>
                    <h2>标题：${resp.title}</h2>
                    `
            }
        }
    }
}
```

## 封装 get 和 post

```javascript
//url,  string 请求的地址
//query,   object 请求携带的参数 
//callback: function  成功之后的回调
//isJson,   boolean 是否需要解析json
function get(url, query, callback, isJson){
    //如果有参数，先把参数拼接在url后面
    if(query){
        url += '?'
        for(var key in query) {
            url += `${key}=${query[key]}&`
        }
        //取出最后多余的 &
        url = url.slice(0,-1)
    }
    var xhr = new XMLHttpRequest()
    xhr.open('get',url)
    xhr.send()
    xhr.onreadystatechange =function(){
        if(xhr.readyState === 4){
            if(xhr.status === 200){
                var res = isJson ? JSON.parse(xhr.responseText) : xhr.responseText
                callback(res)
            }
        }
    }
}
function post(url, query, callback, isJson){
    //如果有参数，把参数拼接起来
    var str = ''
    if(query){
        for(var key in query) {
            str += `${key}=${query[key]}&`
        }
        //取出最后多余的 &
        str = str.slice(0,-1)
    }
    var xhr = new XMLHttpRequest()
    xhr.open('post',url)
    xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded")
    xhr.send(str)
    xhr.onreadystatechange =function(){
        if(xhr.readyState === 4){
            if(xhr.status === 200){
                var res = isJson ? JSON.parse(xhr.responseText) : xhr.responseText
                callback(res)
            }
        }
    }
}

//使用
get('http://localhost:3000/news',{id:2},function (resp){
     console.log(resp)
},true)
```

## 将get post封装到ajax

将所有的参数都封装到一个对象上，调用ajax的时候，传递一个对象就可以，不用再像以前传递多个参数。

```javascript
//params: object : {method,url,query,callback, isJson}
ajax: function (params){
    var xhr = new XMLHttpRequest()
    if(params.method.toLowerCase() === 'get' ){
        if(params.query){
            params.url += '?'
            for(var key in params.query) {
                params.url += `${key}=${params.query[key]}&`
            }
            //取出最后多余的 &
            params.url = params.url.slice(0,-1)
        }
        xhr.open('get', params.url)
        xhr.send()
    }else{
        //post
        if(params.query){
            var str = ''
            for(var key in params.query){
                str += `${key}=${params.query[key]}&` 
            }
            str = str.slice(0,-1)
        }
        xhr.open('post',params.url)
        xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded")
        xhr.send(str)
    }
    xhr.onreadystatechange =function (){
        if(xhr.readyState === 4){
            if(xhr.status === 200){
                var resp = params.isJson?JSON.parse(xhr.responseText):xhr.responseText
                params.callback(resp)
            }
        }
    }
}

//使用，这里是因为ajax函数是在util对象里，所以调用的时候是util.ajax
//get
util.ajax({
    method:'get',
    url:'http://localhost:3000/news',
    query:{id:2},
    callback:function(resp){
        console.log(resp)
    },
    isJson:true
})
//post
util.ajax({
    method:'post',
    url:'http://localhost:3000/news',
    query:{id:2},
    callback:function(resp){
        console.log(resp)
    },
    isJson:true
})
```

# form表单对象

form.elements.length 返回属于表单元素的个数，如input、textarea等等。

form.elements  包含所有表单元素的数组。数组只返回input、select、textarea以及其他表单字段。

elements数组中的每个表单元素都有自己的一组属性。比如，value属性中保存的就是表单元素的当前值：element.value。等价于 element.getAttribute("value")