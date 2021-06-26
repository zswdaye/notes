# VUE2.x

## 创建Vue实例传入的options

**el**: string | HTMLElement(document.getElementById())

作用：决定之后Vue实例会管理哪一个DOM

**data**: Object | Function(组件当中data必须是一个函数)

作用：Vue实例对应的数据对象

**computed**:{[key:string]: Function}

作用：计算属性，和方法很相像，但是它可以缓存，调用一次就可以，而方法每次都要调用一次。

**methods**: {[key:string]: Function}

作用：定义属于Vue的一些方法，可以在其他方面调用，也可以在指令中使用。



## 插值

### Mustache语法

{{}}

mustache语法中，不仅仅可以直接写变量，也可以写简单的表达式

```html
<h2>{{message}}</h2>
<h2>{{message}},大帅哥</h2>
<!-- mustache语法中，不仅仅可以直接写变量，也可以写简单的表达式 -->
<h2>{{firstName + ' ' + lastName}}</h2>
<h2>{{firstName}} {{lastName}}</h2>
<h2>{{counter * 2}}</h2>
```

### v-once指令

v-once后面不需要跟任何表达式

只展示第一次的数据，当数据发生改变时，展示的内容不会发生改变。

```HTML
<h2>{{message}}</h2>
<h2 v-once>{{message}}</h2>
```

### v-html指令

该指令后面往往会跟上一个string类型，会将string的html解析出来并且进行渲染

```html
<h2>{{url}}</h2>
<h2 v-html="url"></h2>
url: '<a href="https://www.baidu.com">百度一下</a>'
```

### v-text指令

该指令和Mustache比较相似，v-text通常情况下，后接一个string类型

***不常用***

```html
<h2>{{message}}</h2>
<h2 v-text="message"></h2>
```

### v-pre指令

将元素里的内容原封不动的显示出来，不进行解析

```html
<h2>{{message}}</h2>
<h2 v-pre>{{message}}</h2>
```

### v-cloak指令

在vue解析之前，div中有一个属性v-cloak

在vue解析之后，div中没有一个属性v-cloak

```html
<style>
    [v-cloak]{
        display: none;
    }
</style>
<div id="app" v-cloak>
    {{message}}
</div>
```

### v-bind指令

动态绑定某些元素的属性，如a标签的href，img的src属性

基本使用：

```html
<!-- 错误的做法：这里不可以使用mustache语法 -->
<!-- <img src="{{imgURL}}" alt=""> -->
<!-- 正确的做法 -->
<img v-bind:src="imgURL" alt="">
<a v-bind:href="aHref">百度一下</a>
```

语法糖就是简写

语法糖写法就是在属性前面加 :

```html
<!-- 语法糖的写法 -->
<img :src="imgURL" alt="">
<a :href="aHref">百度一下</a>
```

#### 动态绑定class

**对象语法**

在 :class里放一个对象，可以动态的决定是否要将某个class给删除掉。而且对于固定的class有一个合并作用，不会影响它的显示。

```html
<!-- <h2 v-bind:class="{类名1:boolean, 类名2: boolean}">{{message}}</h2> -->
<h2 class="title" v-bind:class="{active: isActive, line: isLine}">{{message}}</h2>
<h2 class="title" :class="getClass()">{{message}}</h2>
<button v-on:click="btnClick">按钮</button>
data:{
	message:'你好吗',
    isActive: true,
	isLine: true
},
methods:{
	btnClick:function(){
		this.isActive = !this.isActive
	},
	getClass:function(){
		return {active: this.isActive, line: this.isLine}
	}
}
```

**数组语法**(用的少)

在数组里加单引号就是直接添加字面量，不加单引号就是引用一个变量

```html
<h2 class="title" :class="['active','line']">{{message}}</h2>
<h2 class="title" :class="[active,line]">{{message}}</h2>
<h2 class="title" :class="getClasses()">{{message}}</h2>
data:{
	message:'你好吗',
	active: 'aaaa',
	line: 'bbb'
},
methods:{
	getClasses:function(){
		return [this.active,this.line]
	}
}
```

#### 动态绑定style

属性名可以使用以前的，也可以使用驼峰命名法的属性名

**对象语法**

在属性值上要加单引号，要不然系统会将它当成一个变量

```php+HTML
<!-- <h2 :style="{key(css属性名): value(属性值)}">{{message}}</h2> -->

<!-- '50px'必须加上单引号，否则是当做一个变量去解析 -->
<!-- <h2 :style="{fontSize: '50px'}">{{message}}</h2> -->

<!-- finalSize当成变量使用 -->
<!-- <h2 :style="{fontSize: finalSize}">{{message}}</h2> -->
<!-- <h2 :style="{fontSize: finalSize + 'px', color: finalColor}">{{message}}</h2> -->

<h2 :style="getStyles()">{{message}}</h2>
data:{
	message:'你好吗',
	//   finalSize: '100px',
	finalSize:100,
	finalColor: 'red',
},
methods:{
	getStyles:function(){
		return {fontSize: this.finalSize + 'px', color: this.finalColor}
	}
}
```

**数组语法**(用的少)

```html
<h2 :style="[baseStyle,baseStyle1]">{{message}}</h2>
data:{
	message:'你好吗',
	baseStyle:{backgroundColor: 'red'},
	baseStyle1:{fontSize: '100px'}
}
```

### v-for指令

**遍历数组**

1.在遍历的过程中，没有使用下标值

```vue
<li v-for="(变量,索引) in 列表">{{变量}}</li>
<li v-for="item in movies">{{item}}</li>
data:{
	movies:['海王','海尔兄弟','火影忍者','进击的巨人']
}
```

2.在遍历的过程中，获取索引值

```vue
<ul>
    <li v-for="(item,index) in names">
    <!-- (v,i) in 数组
		  v:数组中的每个元素
		  i:数组中元素的下标
	-->
        {{index+1}}.{{item}}
    </li>
</ul>

data:{
      names:['why','kobe','curry','james']
    }
```

**遍历对象**

1.在遍历对象的过程中，如果只是获取一个值，那么获取到的是value

```vue
<ul>
    <li v-for="item in info">{{item}}</li>
    <!-- (v,k,i)in 对象
		  v:值
		  k:键
		  i:对象中每对key-value的索引 从0开始
		  注意: v,k,i是参数名,见名知意即可!
	-->
</ul>
data:{
      info: {
          name:'why',
          age:18,
          height: 1.88
      }
    }
```

2.获取key和value (value, key)

```vue
<ul>
    <li v-for="(value, key) in info">{{value}}-{{key}}</li>
</ul>
```

3.获取key和value和index 格式：(value, key, index)

```vue
<ul>
	<li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
</ul>
```

> 注意: 在使用v-for时,要把一个唯一值赋值给:key属性(通常是数组的index或者数据的id)

```js
<div id="app">
   <!-- v-for 
        key属性: 值通常是一个唯一的标识
        key是一个可选属性
        养成好习惯:建议在写v-for时 设置:key="唯一值"
   -->
   <ul>
     <li v-for="(item,index) in list" :key="index">{{item}}---{{index}}</li>
   </ul>
</div>
<script src="./vue.js"></script>
<script>
   new Vue({
      el: '#app',
      data: {
        list: ['a', 'b', 'c']
      },
      methods: {
	  }
   });
</script>
```

### v-on指令

作用：绑定事件监听器

缩写：@

基本使用：

```vue
<!-- <button v-on:click="count++">+</button>
<button v-on:click="count--">-</button> -->
<!-- <button v-on:click="increment">+</button>
<button v-on:click="decrement">-</button> -->
<button @click="increment">+</button>
<button @click="decrement">-</button>
methods:{
	increment(){
		this.count++
	},
	decrement(){
		this.count--
	}
}
```

#### **传入参数**

1.事件调用的方法没有参数

```vue
<button @click="btn1Click()">按钮1</button>
<button @click="btn1Click">按钮1</button>
```

2.在事件定义时，写参数时省略了小括号，但是方法本身是需要一个参数的,这个时候，

  vue会默认将浏览器产生的event事件对象作为参数传入到方法

```vue
<button @click="btn2Click(123)">按钮2</button>
<button @click="btn2Click">按钮2</button>
```

3.方法定义时，我们需要event对象，同时又需要其他参数

在调用方式，如何手动的获取到浏览器参数的event对象：$event

```vue
<button @click="btn3Click(123,$event)">按钮3</button>
```

#### **修饰符**

1.  .stop修饰符的使用，阻止冒泡行为

   ```vue
   <div @click="divClick">
           aaaaa
         <button @click.stop="btnClick">按钮</button>
       </div>
   ```

   

2.   .prevent修饰符的使用，阻止浏览器的默认行为

   ```vue
   <input type="submit" value="提交" @click.prevent="submitClick">
   ```

3.   监听某个键盘的键帽，这里是回车键

   ```vue
   <input type="text" @keyup.enter="keyup">
   ```

4.  .once修饰符的使用,只触发一次回调

   ```vue
   <button @click.once="btn2Click">按钮2</button>
   ```

### v-if  v-else-if和 v-else指令

```vue
<h1 v-if="true">{{message}}</h1>
<h1 v-if="isShow">{{message}}</h1>
data:{
      message:'你好吗',
      isShow: true
    }
```

```vue
<h2 v-if="score>=90">优秀</h2>
<h2 v-else-if="score>=80">良好</h2>
<h2 v-else-if="score>=60">及格</h2>
<h2 v-else>不及格</h2>
data:{
      score: 85
    },
```

### v-show指令

决定元素是否在页面上渲染显示，和 v-if 很相似

**v-show和v-if的差别**

v-if：当条件为false时，包含v-if指令的元素，根本就不会存在dom中

v-show：当条件为false时，v-show只是给我们的元素添加一个行内样式：display: none

如何选择：

当需要显示与隐藏之间切换很频繁时，使用v-show

当只有一次切换时，使用v-if

```vue
<h2 v-if="isShow" id="aaa">{{message}}</h2>
<h2 v-show="isShow" id="bbb">{{message}}</h2>
data:{
      message:'你好吗',
      isShow: true
    }
```

### v-model指令

一般是和input结合使用，也可以用于textarea

#### 数据双向绑定

数据更改，input输入框的值随之改变；input输入框的值改变，数据也会随之改变

```vue
<input type="text" v-model="message">
    {{message}}
data:{
      message:'你好吗'
    }
```

#### 结合radio类型

使用v-model可以省略掉name，一样可以使单选按钮互斥

可以在data上的sex设置值，来确定默认值

```vue
<label for="male">
	<input type="radio" id="male" value="男" v-model="sex">男
</label>
<label for="female">
	<input type="radio" id="female" value="女" v-model="sex">女
</label>
data:{
      sex: '男'
    }
```

#### 结合checkbox类型

```vue
<!-- 1.checkbox单选框 -->
<label for="agree">
   <input type="checkbox" id="agree" v-model="isAgree">同意协议
</label>
<h2>您选择的是：{{isAgree}}</h2>
<button :disabled="!isAgree">下一步</button>
<br>
<!-- 2.checkbox多选框 -->
<input type="checkbox" value="篮球" v-model="hobbies">篮球
<input type="checkbox" value="乒乓球" v-model="hobbies">乒乓球
<input type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
<input type="checkbox" value="足球" v-model="hobbies">足球

<h2>您的爱好是：{{hobbies}}</h2>

data:{
      isAgree: false,  //单选框
      hobbies: []       //多选框
    }
```

#### 结合select类型

不常用

```vue
<!-- 1.选择一个 -->
    <select name="abc" v-model="fruit">
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="榴莲">榴莲</option>
        <option value="葡萄">葡萄</option>
    </select>
    <h2>您选择的水果是：{{fruit}}</h2>
    <!-- 2.选择多个 -->
    <select name="abc" v-model="fruits" multiple>
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="榴莲">榴莲</option>
        <option value="葡萄">葡萄</option>
    </select>
    <h2>您选择的水果是：{{fruits}}</h2>
data:{
      fruit:'香蕉',
      fruits: []
    }
```

#### input值绑定

```vue
<label v-for="item in originHobbies" for="item">
    <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
</label>
data:{
      hobbies: [],       //多选框
      originHobbies: ['篮球', '足球', '乒乓球', '羽毛球', '台球']
    }
```

#### 修饰符

.lazy修饰符

懒加载，输入框失去焦点的时候显示数据，不再实时显示

```vue
<input type="text" v-model.lazy="message">
<h2>{{message}}</h2>
data:{
      message:'你好吗',
      age: '',
      name: ''
    }
```

.number修饰符

输入框里值其实都是string类型的，可以使用.number将类型转换为number

```vue
<input type="number" v-model.number="age">
<h2>{{age}}-{{typeof age}}</h2>
```

.trim修饰符

去除输入框首尾空格，虽然在浏览器中没有显示出来，但其实 空格是真实存在的

```vue
<input type="text" v-model.trim="name">
<h2>您输入的名字是：{{name}}</h2>
```

### 自定义指令

- 使用场景:需要对普通 DOM 元素进行操作，这时候就会用到自定义指令 
- 分类:全局注册和局部注册

#### 全局注册

1. 在创建 Vue 实例之前定义全局自定义指令Vue.directive()
2. Vue.directive('指令的名称',{ inserted: (使用指令的DOM对象) => { 具体的DOM操作 } } );
3. 在视图中通过"v-自定义指令名"去使用指令

```js
<div id="app">
    <!-- 3. 在视图中通过标签去使用指令 -->
    <input v-focus type="text">

</div>
<script src="./vue.js"></script>
<script>
    // 全局自定义指令
    // 1.在创建 Vue 实例之前定义全局自定义指令Vue.directive()
    // 2. Vue.directive('指令的核心名称',{ inserted: (使用指令时的DOM对象) => { 具体的DOM操作 } } );
    Vue.directive('focus', {
        // 当被绑定的元素插入到 DOM 中时,inserted会被调用，inserted是钩子函数
        inserted: (el) => {
            // el 就是指令所在的DOM对象
            el.focus();
        }
    });
   //解释钩子函数
	Vue.directive('my-directive', {
        bind: function () {
        // 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
        // 比如添加事件监听器，或是其他只需要执行一次的复杂操作
        },
        inserted: function () {
        // 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
        },
        update: function () {
        // 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。
        },
        componentUpdated: function () {
        // 指令所在组件的 VNode 及其子 VNode 全部更新后调用。
        },
        unbind: function () {
        // 只调用一次，指令与元素解绑时调用。
        // 比如移除在 bind() 中添加的事件监听器
        }
     })
    var vm = new Vue({
        el: '#app'
    });
</script>
```

#### 局部注册

1. 在vm对象的选项中设置自定义指令 directives:{}
2. directives:{'指令的核心名称':{ inserted: (使用指令时的DOM对象) => { 具体的DOM操作 } }}
3. 在视图中通过标签去使用指令

```js
<div id="app">
    <!-- 3. 在视图中通过标签去使用指令 -->
    <input v-focus type="text">
</div>

<script src="./vue.js"></script>
<script>
    var vm = new Vue({
        el: '#app',
        // 1. 在vm对象的选项中设置自定义指令 directives:{}

        directives: {
            // 2. directives:{'指令的核心名称':{ inserted: (使用指令时的DOM对象) => { 具体的DOM操作 } }}
            'focus': {
                // 指令的定义
                inserted: function(el) {
                    el.focus();
                }
            }
        }
    });
</script>
```

## 过滤器

- 作用:处理数据格式
- 使用位置:**双花括号插值和 v-bind 表达式** (后者从 2.1.0+ 开始支持)。
- 可以叠加使用过滤器{{ 数据 | 过滤器的名字1 | 过滤器的名字2 }}
- 分类:局部注册和全局注册

### 全局注册

1. 在创建 Vue 实例之前定义全局过滤器Vue.filter()
2. Vue.filter('该过滤器的名字',(要过滤的数据)=>{return 对数据的处理结果});
3. 在视图中通过{{数据 | 过滤器的名字}}或者v-bind使用过滤器

```js
<div id="app">
    <!-- 3. 调用过滤器: (msg会自动传入到toUpper中)-->
    <p>{{msg | toUpper}}</p>
</div>
<script src="./vue.js"></script>
<script>
    // 1. 定义全局过滤器,value是指过滤器前面的数据msg，后面也可以添加其他参数
    Vue.filter('toUpper', (value) => {
        console.log(value);
        // 2. 操作数据并返回
        value = value.charAt(0).toUpperCase() + value.substr(1).toLowerCase();
        console.log(value);
        return value;
    });

    new Vue({
        el: '#app',
        data: {
            msg: 'hello'
        },
        methods: {

        }
    });
</script>
```

### 局部注册

1. 在vm对象的选项中配置过滤器filters:{}
2. 过滤器的名字: (要过滤的数据)=>{return 过滤的结果}
3. 在视图中使用过滤器:  {{被过滤的数据 | 过滤器的名字}}

```js
<div id="app">
    <!-- 3. 调用过滤器 -->
    <p>{{ msg | upper }}</p>
</div>
<script src="./vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'kfc'
        },
        // 1. 设置vm的过滤器filters选项
        filters: {
            upper: function(v) {
                // 2. 在过滤器的方法中操作数据并返回结果
                return v.toUpperCase();
            }
        }
    });
</script>
```

## 侦听器

作用：数据发生变化时调用

```js
<h2>{{num}}</h2>
data:{
  num:0,
  isMax:false,//数量不超过10
},
watch:{
    /* 侦听num属性的数据变化
	   作用：num的值一旦发生改变，这个方法就会被调用
	   newVal, 新值
	   oldVal：旧的值，之前的值					
	*/
    num(newVal,oldVal){
       if(newVal<0){
		  this.num=0;
	    }
		if(newVal>10){
		  this.isMax=true
		  alert('库存不足')
		}
    }
}
```

```js
data:{
	users:{
		name:'12'
	},
	names:'aa'
},
watch:{
	// 数组和对象都是引用类型，引用类型的变量存储的是地址,所以不能直接侦听
	// name(news,old){
	// 	console.log(news,old)
	// }
	// 第一种方式：侦听整个对象，每个属性值变卦都会执行handler
	// users:{
	// 	/*handler 就是原来的侦听函数，当数据发生变化会执行该函数*/
	// 	handler(news,old){
	// 		console.log(news.name,old.name)
	// 		console.log(news)
	// 	},
	// 	// immediate 代表watch里面申明了一次，理解为进入页面，就执行监听函数handler
	// 	immediate:false,
	// 	// deep 属性代表是否深度监听，默认是false,当值为true的时候会对对象每个属性进行侦听
	// 	deep:true					
	// },
	names:{
		handler(n,o){
			console.log(n)
		},
		immediate:true
	},
	// 第二种方式  ：侦听对象的某一个属性,值发生变化就会执行
	'users.name' (news,old){
		console.log(news,old)
	}
}
```



## 计算属性

将多个数据结合起来显示

```vue
<h2>{{fullName}}</h2>
data:{
	firstName: 'Lebron',
	lastName: 'James'
},
//computed:计算属性
computed:{
	fullName:function(){
		return this.firstName + ' ' + this.lastName
	}
},
```

### setter和getter属性

计算属性一般是没有set方法的，因为大多数都是只要只读就行。set方法不常用

```vue
computed: {
// 计算属性一般是没有set方法，只读属性
	fullName:{
		set: function(newValue){
			console.log('-----',newValue);
			const names = newValue.split(' ');
			this.firstName = names[0];
			this.lastName = names[1];
		},
		get: function(){
			return this.firstName + ' ' + this.lastName
		}
	}
}
```

因为只要只读属性，所以也可以写成

```vue
fullName: function(){
     return this.firstName + ' ' + this.lastName
}
```

### 计算属性和methods的对比

计算属性拥有缓存，在没有改变属性的情况下，执行多次相同的代码时，只调用一次

methods没有缓存，执行几次调用几次。

## 数组方法为响应式的

1.push()

​	- 在数组最后添加元素

```vue
data:{
      letters: ['a','b','c','d']
    //   letters: [2,1,1,5,4]
    },
this.letters.push('aaa')
this.letters.push('aaa','bbb','ccc')
```

2.pop()

​	- 删除数组中的最后一个元素

```vue
this.letters.pop()
```

3.shift()

​	- 删除数组中的第一个元素

```vue
this.letters.shift()
```

4.unshift()

​	- 在数组最前面添加元素

```vue
this.letters.unshift('aaaa')
this.letters.unshift('aaaa','bbbb','cccc')
```

5.splice()

​	- 删除元素/插入元素/替换元素

​	- 删除元素：第二个元素传入你要删除几个元素(如果没有传，就删除后面所有的元素)

​	- 替换元素：第二个参数，表示我们要替换几个元素，后面是用于替换前面的元素

​	- 插入元素：第二个参数，传入0，并且后面跟上要插入的元素

```vue
this.letters.splice(1, 3, 'm', 'n', 'l')
this.letters.splice(1, 0, 'm', 'n', 'l')
```

6.sort()

​	- 排序

```vue
this.letters.sort()
```

7.reverse()

​	- 反转

```vue
this.letters.reverse()
```

**注意：通过索引值修改数组中的元素，不是响应式的**

```vue
this.letters[0]='bbbbb'
```

## 数组高阶函数

### filter

filter中的回调函数有一个要求：必须返回一个boolean值

​	- true：当返回true时，函数内部会自动将这次回调的n加入到新的数组中

​	- false：当返回false时，函数内部会过滤这次的n

```javascript
const nums = [10,20,105,204,625,84,52]

let newNums = nums.filter(function (n){
    return n < 100
})
```

### map

map会将返回值加入到新的数组中

```javascript
let new2Nums = newNums.map(function (n){
    return n*2
})
```

### reduce

reduce作用对数组中所有的内容进行汇总, 返回一个值

reduce的回调函数传入两个值：

​	- preValue代表的是前一个汇总的结果，第一次的preValue默认是0，如果回调函数后面的逗号 , 有值，则第一次的preValue的值就是那个值

​	- n 代表的是数组里的每个值

```javascript
let total = new2Nums.reduce(function (preValue, n){
    return preValue + n
}, 0)
```

```javascript
const nums = [10,20,105,204,625,84,52]
let total = nums.filter(function(n){
    return n<100
}).map(function (n){
    return n *2
}).reduce(function(preValue, n){
    return preValue + n
},0)
```

## 组件化

组件化基本使用

1.创建组件构造器对象

```vue
const cpnC = Vue.extend({
        template: `
        <div>
          <h2>我是标题</h2>
          <p>我是内容，哈哈哈哈</p>
          <p>我是内容，呵呵呵呵</p>
        </div>`
    })
```

2.注册组件

Vue.component('组件标签名',组件构造器)

```vue
Vue.component('my-cpn',cpnC)
```

3.使用组件

一定要在Vue挂载的实例上使用

```vue
<my-cpn></my-cpn>
```

### 全局组件和局部组件

上面的做法就是定义了一个全局组件，任何一个挂载了Vue实例的都可以使用

**局部组件的使用**

将上面的第二步 (注册组件) 给注释掉

在指定Vue实例上进行组件的注册，这样注册的组件只能在特定的Vue实例上使用

```vue
const app = new Vue({
    el:'#app',
    data:{
      message:'你好吗'
    },
    components: {
        //cpn使用组件时的标签名
        cpn: cpnC
    }
  })
```

**全局组件的语法糖**

```vue
Vue.component('cpn1', {
    template: `
        <div>
          <h2>我是标题</h2>
          <p>我是内容，哈哈哈哈</p>
        </div>
      `
  })
```

**局部组件的语法糖**

```vue
const app = new Vue({
    el:'#app',
    data:{
      message:'你好吗'
    },
    components: {
        'cpn2': {
            template: `
                <div>
                <h2>我是标题</h2>
                <p>我是内容，哈哈哈哈</p>
                </div>
            `
        }
    }
```



### 父组件和子组件

创建两个组件构造器，然后在父组件中将子组件进行注册。父组件中的template里可以使用子组件

***注意：如果子组件没有在全局注册或者在Vue实例里注册，是不能使用的***

```vue
const cpnC1 = Vue.extend({
    template: `
        <div>
          <h2>我是标题</h2>
          <p>我是内容，哈哈哈哈</p>
        </div>
      `
  })
const cpnC2 = Vue.extend({
    template: `
        <div>
          <h2>我是标题2</h2>
          <p>我是内容，哈哈哈哈</p>
          <cpn1></cpn1>
        </div>
      `,
    components: {
        cpn1: cpnC1
    }
  })
const app = new Vue({
    el:'#app',
    data:{
      message:'你好吗'
    },
    components: {
        cpn2: cpnC2
    }
  })
```

### 组件模板分离(template)

在组件构造，注册的时候，template里面的内容太多，会容易导致写错，所以将template分离出来。

1.利用script标签进行分离，

```vue
<!-- 添加id属性用来将里面的内容给传递过去 -->
<script type="text/x-template" id="cpn">
    <div>
        <h2>我是标题</h2>
        <p>我是内容，哈哈哈</p>
    </div>
</script>
Vue.component('cpn',{
	  //引用上面的id
      template: '#cpn'
  })
```

2.使用template标签来进行分离

```vue
<template id="cpn">
    <div>
        <h2>我是标题</h2>
        <p>我是内容，呵呵呵</p>
    </div>
</template>
Vue.component('cpn',{
	  //引用上面的id
      template: '#cpn'
  })
```

### 组件数据

组件不能使用Vue实例中的数据

组件对象也有一个data属性，data属性必须是一个函数，返回值是一个对象。这样组件模板里就可以使用自己的数据

```vue
<template id="cpn">
    <div>
        <h2>我是标题</h2>
        <h3>{{title}}</h3>
        <p>我是内容，呵呵呵</p>
    </div>
</template>
Vue.component('cpn',{
      template: '#cpn',
      data(){
          return {
              title: 'abc'
          }
      }
  })
```

## 父子组件之间的通讯

### 父传子props

在注册子组件的时候，有个props 的options，里面存放变量

```vue
//父组件
data:{
      message:'你好吗',
      movies: ['海王', '火影忍者', '海尔兄弟', '海贼王']
    },
    components: {
        cpn
    }
<template id="cpn">
    <div>
        <ul>
            <li v-for="item in cmovies">{{item}}</li>
        </ul>
        <h2>{{cmessage}}</h2>
    </div>
</template>
//子组件
const cpn = {
      template: '#cpn',
	//	数组方式的通讯
    //   props: ['cmovies', 'cmessage'],
    //  对象方式的通讯
      props: {
          //1.类型的限制
        //   cmovies: Array,
        //   cmessage: String

          //2.提供一些默认值,以及必传值
          cmessage: {
              type: String,
              default: 'aaa',
              required: true
          },
          //类型是对象或者数组时，默认值必须是一个函数
          cmovies: {
              type: Array,
              default() {
                  return []
              }
          }
      },
      data(){
          return {}
      }
  }
<cpn :cmessage="message" :cmovies="movies"></cpn>
```

props中最好不要使用驼峰命名，如要使用，只能如下

```vue
<cpn :c-info="info" :child-message="message"></cpn>
<template id="cpn">
    <div>
        <h2>{{cInfo}}</h2>
        <p>{{childMessage}}</p>
    </div>    
</template>
const cpn = {
      template: '#cpn',
      props: {
          cInfo: {
              type: Object,
              default(){
                  return {}
              }
          },
          childMessage: {
              type: String,
              default: ''
          }
      }
  }
```

### 子传父 $emit  .sync

利用函数，将子组件上的信息传递给父组件

```vue
 <!-- 父组件模板 -->
<div id="app">
    <!-- 使用$emit进行子传父 -->
    <cpn @itemclick="cpnClick"></cpn>
    <!-- 使用.sync修饰符进行子传父 -->
    <cpn :itemclick.sync="cpnClick"></cpn>
</div>
<!-- 子组件模板 -->
<template id="cpn">
    <div>
        <button v-for="item in categories" @click="btnClick(item)">
            {{item.name}}
        </button>
    </div>    
</template>
//子组件
  const cpn = {
    template: '#cpn',
    data(){
        return {
            categories: [
                {id: 'aaa', name: '热门推荐'},
                {id: 'bbb', name: '手机数码'},
                {id: 'ccc', name: '家用电器'},
                {id: 'ddd', name: '电脑办公'},
            ]
        }
    },
    methods:{
        btnClick(item){
            //使用$emit发射
            this.$emit('itemclick', item)
			//使用.sync修饰符
			this.$emit('update:itemclick', item)
        }
    }
  }
//   父组件
  const app = new Vue({
    el:'#app',
    data:{
      message:'你好吗'
    },
    components: {
        cpn
    },
    methods: {
        cpnClick(item){
            console.log(item.name)
        }
    }
  })
```

## 父子组件的访问方式

### 父访问子 $children

$children可以拿到所有的子组件，并且可以通过这个拿到子组件的属性，方法。（不常用）

```vue
<div id="app">
    <cpn></cpn>
    <cpn></cpn>
    <cpn></cpn>
    <button @click="btnClick">按钮</button>
  </div>
<template id="cpn">
      <div>我是一个子组件</div>
  </template>
const app = new Vue({
    el:'#app',
    data:{
      message:'你好吗'
    },
    methods: {
        btnClick(){
            console.log(this.$children);
            for (let c of this.$children) {
                console.log(c.name);
                c.showMessage()      
            }
        }
    },
    components:{
            cpn: {
                template: '#cpn',
                data(){
                    return {
                        name: '我是子组件的name'
                    }
                },
                methods: {
                    showMessage(){
                        console.log('showMessage')
                    }
                },
            }
        }
})
```

### 父访问子 $refs

$refs => 对象类型，默认是一个空的对象，要在组件上添加 **ref** 属性，可以取到特定的子组件

```vue
<cpn ref="aaa"></cpn>
<button @click="btnClick">按钮</button>
methods: {
    btnClick(){
          console.log(this.$refs.aaa);
    }
}
```

### 子访问父 $parent

$parent 可以拿到父组件，并且可以拿到父组件的属性，方法。（不常用，和父组件耦合比较高）

```vue
<div id="app">
    <cpn></cpn>
  </div>
<template id="cpn">
    <div>
        <h2>我是cpn组件</h2>
        <ccpn></ccpn>
    </div>
    
</template>

<template id="ccpn">
    <div>
        <h2>我是一个子组件</h2>
        <button @click="btnClick">按钮</button>
      </div>
</template>
components:{
        cpn: {
            template: '#cpn',
            data(){
                return {
                    name: '我是cpn组件的name'
                }
            },
            components: {
                ccpn: {
                    template: '#ccpn',
                    methods: {
                        btnClick(){
                            // 1.访问父组件 $parent
                            console.log(this.$parent);    //VueComponent
                            console.log(this.$parent.name);   //我是cpn组件的name
                        }
                    },
                }
            }
        }
    }
```

### 子访问父 $root (根组件)

$root 访问根组件 （用的比较少）

```vue
methods: {
                        btnClick(){
                            // 1.访问父组件 $parent
                            // console.log(this.$parent);
                            // console.log(this.$parent.name);

                            // 2.访问根组件 $root
                            console.log(this.$root);   //Vue
                            console.log(this.$root.message); //你好吗
                        }
                    },
```

## 组件插槽 slot

在template中添加 slot标签，如果想扩展内容，就在cpn组件中添加你想扩展的内容

```vue
<div id="app">
    <cpn><button>按钮</button></cpn>
    <cpn>
        <span>我是span</span>
    	<p>呵呵呵</p>
    </cpn>
    <cpn><i>我是i</i></cpn>
</div>

<template id="cpn">
      <div>
          <h2>我是组件</h2>
          <p>我是组件，哈哈哈</p>
          <slot></slot>
      </div>
  </template>
```

在slot中也可以写默认内容，在cpn中如果不添加内容，就会默认使用slot中的内容

```vue
<div id="app">
    <cpn></cpn>
    <cpn><span>我是span</span></cpn>
    <cpn>
        <i>我是i</i>
        <p>呵呵呵</p>
    </cpn>
    <cpn></cpn>
    <cpn></cpn>
  </div>

<template id="cpn">
      <div>
          <h2>我是组件</h2>
          <p>我是组件，哈哈哈</p>
          <slot><button>按钮</button></slot>
      </div>
  </template>
```

### 具名插槽

给插槽 slot 加上一个名字，使用组件的时候，如果组件添加的内容没有加上属性 slot="插槽名"，就不会替换有插槽名的插槽里默认内容，只会替换没有插槽名的插槽里的内容

```vue
<div id="app">
    <cpn><span slot="center">标题</span></cpn>
    <cpn><i>呵呵呵</i></cpn>
</div>
<template id="cpn">
    <div>
        <slot name="left"><span>左边</span></slot>
        <slot name="center"><span>中间</span></slot>
        <slot name="right"><span>右边</span></slot>

        <slot><span>hahaha</span></slot>
    </div>
</template>
```

### 作用域插槽

父组件替换插槽的标签，但是内容由子组件来提供

```vue
<div id="app">
     <cpn></cpn>
     <cpn>
         <!-- 目的是获取子组件中的pLanguages ，2.5版本以下是必须使用template
				2.6.0以上不再使用slot-scope
		 -->
         <template slot-scope="slot">
				<!-- 这里的 slot.data 的data要和下面的命名一样 -->
             <span v-for="item in slot.data">{{item}} - </span>
             <span>{{slot.data.join(' - ')}}</span>
         </template>
         
     </cpn>
     <cpn>
        <template slot-scope="slot">
            <!-- <span v-for="item in slot.data">{{item}} * </span> -->
            <span>{{slot.data.join(' - ')}}</span>
        </template>
    </cpn>
    <!-- 2.6.0以上的使用作用域插槽的方法 -->
    <cpn>
		<!-- 使用v-slot接收具名插槽data的数据,并把他命名成 bb(名称是自定义) -->
		<template v-slot="bb">
			<span>{{bb.data.join(' - ')}}</span>
		<!-- 如果具名插槽的名字为aaa，则使用方法是
			<span>{{bb.aaa.join(' - ')}}</span>
		-->	
		</template>
	</cpn>
</div>
<!-- 子组件的模板 -->
<template id="cpn">
    <div>
        <!-- :data这个名字可以随便起，:aaa都可以,但要和上面的一样 -->
        <slot :data="pLanguages">
            <ul>
                <li v-for="item in pLanguages">{{item}}</li>
            </ul>
        </slot>
    </div>
</template>

components: {
        cpn: {
            template: '#cpn',
            data(){
                return {
                    pLanguages: ['JavaScript', 'Java', 'Python', 'C++', 'Swift', 'C#']
                }
            }
        }
     }
```

## ES6  模块化使用

### 导出

```javascript
let name = '小明'
let age = 18
let flag = true

function sum(a, b){
    return a + b;
}
if(flag){
    console.log(sum(20, 30));
}
//1.导出方式一；
export {
    flag, 
    sum
}
//2.导出方式二；
export var num1 = 1000;
export var height = 1.88
//3.导出函数/类
export function mul(num1, num2){
    return num1 * num2
}
export class Person {
    run(){
        console.log('在奔跑')
    }
}
// 5.export default 导入的时候名字可以随便起，而且不需要{}
// 在同一个模块中，不允许多个export default，只能有一个
// const address = '北京市'
// export default address

export default function (argument) {
    console.log(argument);
}
```

### 导入

```javascript
//1.导入的{}中定义的变量
import {flag,sum} from './aaa.js'

if(flag){
    console.log('小明是天才，哈哈');
    console.log(sum(30, 50))
}
//2.直接导入export定义的变量
import {num1, height} from './aaa.js'

console.log(num1,height)
//3.导入export的function/class
import {mul, Person} from './aaa.js'

console.log(mul(100, 200))
const p = new Person()
p.run()
//4.导入 export default中的内容
import addr from './aaa.js'
addr('你好啊')

//5.统一全部导入,起个别名aaa ，然后通过aaa对象来引用里面的东西
import * as aaa from './aaa.js'

console.log(aaa.flag)
```

## webpack

### 初步使用

既可以使用commonjs的模块化规范，也可以使用ES6的模块化的规范

commonjs模块化

```javascript
//导出
function add(num1, num2){
    return num1 + num2
}
function mul(num1, num2){
    return num1 * num2
}

module.exports = {
    add,
    mul
}
//导入
const {add,mul} =require('./mathUtils.js')

console.log(add(20, 30))
console.log(mul(20, 30))
```

安装webpack，这里使用的是3.6.0版本。

将需要使用的文件在项目入口文件 main.js 引入。

在控制台上，cd 到项目目录上，webpack 要打包的文件入口.js 打包后的文件.js，

如：(webpack ./src/main.js ./dist/bundle.js)  这样就算打包成功了，可以直接引用bundle.js

### webpack配置

上面打包方式太过于繁琐，所以增加新的配置，来完善。

先在目录上新建一个webpack.config.js文件，将项目要打包的文件入口和出口 定义好。

```javascript
const path = require('path')

module.exports = {
    entry: './src/main.js',
    output: {
        // path: 动态获取路径,
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
}
```

出口要绝对路径，所以下面的第一步操作是为了动态获取路径。

1.在控制台，cd 到项目目录上，npm init    

​	- 初始化项目，在项目目录上会增加一个package.json文件

2.接着 npm install

​	- 会将package.json中的依赖安装起来，当前package.json没有依赖，所以没有安装什么。并同时生成了一个新的文件package-lock.json

3.webpack   就可以将项目打包了

​	- 这样的打包是通过全局来打包的

全局的版本可能和项目的webpack版本不一致，可能会导致打包出问题，所以一般都是用局部的webpack打包。

在package.json文件的scripts下添加  "build": "webpack"，在终端上运行 npm run build 也可以进行打包，这样的打包方式是**优先**局部的(本地的)。

在终端上 npm install webpack@3.6.0 --save-dev 会在项目上安装局部的(本地的)webpack。用局部的webpack方式打包项目也很繁琐，如：(./node_modules/.bin/webpack)。所以在终端上运行npm run build这样的打包方式是最优的。

### loader处理css，less等等

css，less这种文件要在项目入口 main.js 引入

针对css，less，图片这种文件，webpack打包会出现错误，所以需要loader来进行处理。不同的文件需要不同的loader

第一步：npm install相关的loader

第二步：在 webpack.config.js 中进行配置

可以去官网查看不同文件的loader的安装和配置

**踩坑**：由于当前使用的webpack是3.6.0，安装css-loader的版本太高，会导致运行出错，less相同，以此类推。

npm install --save-dev css-loader@2.0.2

npm install --save-dev less-loader@4.1.0 less@3.9.0

npm install --save-dev url-loader@1.1.2

npm install --save-dev file-loader@3.0.1

配置图片，先下载 url-loader。配置的时候，当加载的图片，小于limit时，会将图片编译成base64字符串形式，大于的时候会去找 file-loader，所以也要下载file-loader，以及要在 webpack.config.js 的output上进行配置.

```javascript
output: {
        // path: 动态获取路径,
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
        //在这里添加
        publicPath: 'dist/'
    },
```

为了能够更好的管理图片，我们需要在 webpack.config.js 配置url-loader的时候，添加一些配置

```javascript
{
    test: /\.(png|jpg|gif)$/,
    use: [
       {
          loader: 'url-loader',
          options: {
             //当加载的图片，小于limit时，会将图片编译成base64字符串形式
             //当加载的图片，小于limit时，需要使用file-loader模块进行加载
             limit: 8192,
              //在打包处会添加一个img文件夹，里面包含图片原来的名字.8位hash值.扩展名
             name: 'img/[name].[hash:8].[ext]'
          }                        
       }
    ]
}
```

#### ES6转ES5的babel

npm install --save-dev babel-loader@7 babel-core babel-preset-es2015

配置 webpack.config.js

```javascript
{
    test: /\.js$/,
    //exclude: 排除
    exclude: /(node_modules|bower_components)/,
    use: {
       loader: 'babel-loader',
       options: {
           presets: ['es2015']
       }
    }
}
```

### webpack配置vue

在项目目录下 npm install vue --save

在 main.js 中引入vue       import Vue from 'vue'

在main.js中编写vue代码

运行时发现报错，是因为使用了runtime-only版本，该版本不允许出现template。

解决方案：在 webpack.config.js 的module.exports里新建一个resolve

```javascript
resolve: {
        //alias: 别名
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    }
```

1.在index.html文件中不应有太多代码，所以将 <div id="app"></div> 里面的东西都放在main.js 的vue实例里的 template 里。

2.但将东西都放在 template 里会显得vue实例太长，不好维护，将template里的东西、data 、methods都抽出去，抽成一个组件

```javascript
import Vue from 'vue'
import App from './vue/App.vue'

new Vue({
    el: '#app',
    template: '<App/>',
    components: {
        App
    }
})
```

为了使用.vue文件，需要配置vue的loader

npm install vue-loader vue-template-compiler --save-dev

vue-loader 的版本太高会出错，在package.json文件中修改版本为 13.0.0，npm install即可

3.在App.vue上编写代码

```vue
<template>
    <div>
        <h2 class="title">{{message}}</h2>
        <button @click="btnClick">按钮</button>
        <p>{{name}}</p>
        <Cpn />
    </div>
</template>

<script>
//引入子组件
import Cpn from './Cpn'

export default {
    name: 'App',
    components: {
        Cpn
    },
    data(){
        return {
            message: 'Hello world',
            name: 'codewhy'
        }
    },
    methods: {
        btnClick(){

        }
    },
}
</script>

<style scoped>
  .title{
      color: green;
  }
</style>
```

4.在子组件Cpn.vue上编写的代码

```vue
<template>
    <div>
        <h2>我是cpn组件的标题</h2>
        <p>我是cpn组件的内容，哈哈哈</p>
    </div>
</template>

<script>
export default {
    name: "Cpn",
    data(){
        return {
            name: 'Cpn组件的name'
        }
    }
}
</script>
```

在引用.vue文件时需要加入扩展名.vue，为了能够省略，可以在 webpack.config.js上进行配置，在resolve上就可以进行配置

```js
resolve: {
    //省略扩展名
    extensions: ['.js', '.vue'],
        //alias: 别名
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
}
```

### 插件的使用

#### 版权插件

不需要安装插件，这是内置插件

在webpack.config.js 的module.exports上添加plugins

```js
plugins: [
        new webpack.BannerPlugin('最终版权归aaa所有')
    ]
```

打包的时候，会自动在出口文件上方添加版权声明的注释

#### 打包HTML的插件

自动生成打包html文件

1.安装插件

npm install html-webpack-plugin@3.2.0 --save-dev    

2.在webpack.config.js 中引入插件  const HtmlWebpackPlugin = require('html-webpack-plugin')

在plugins中添加插件

```js
plugins: [
        new webpack.BannerPlugin('最终版权归aaa所有'),
    	//这个就是html插件，在打包的时候自动生成index.html文件
        new HtmlWebpackPlugin({
            //这是指定一个模板，在生成的index.html文件中会添加模板index.html里面的内容
            template: 'index.html'
        })
    ]
```

为了生成的index.html文件可以正确的引入bundle.js，我们将前面在 webpack.config.js 里的output里面定义的 publicPath: 'dist/' 给注释掉。为了生成的index.html文件里面有我们需要的内容，我们将模板里的 script 给删掉。

#### js压缩插件

1.安装插件

npm install uglifyjs-webpack-plugin@1.1.1 --save-dev

2.在webpack.config.js 中引入插件  const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

在plugins中添加插件

```js
plugins: [
        new webpack.BannerPlugin('最终版权归aaa所有'),
        new HtmlWebpackPlugin({
            template: 'index.html'
        }),
        //压缩js插件
        new UglifyjsWebpackPlugin()
    ]
```

### 搭建本地服务器

在以后开发的过程中，如果一直使用上面的命令运行来查看效果，是很耗时的，每次都要打包一次，项目越大，越耗时。为了节约时间，我们要搭建本地服务器，代码一修改，我们就能实时的查看到效果，等到要发布的时候，我们再一起打包就可以了。

1.安装

npm install --save-dev webpack-dev-server@2.9.3

2.配置

在webpack.config.js 的module.exports上添加devServer，devServer有如下属性：

contentBase: 为哪个文件夹提供本地服务，默认是根文件夹，我们这里要填写./dist

port：端口号

inline：页面实时刷新

historyApiFallback：在SPA页面中，依赖HTML5的history模式

3.跑起来

在终端运行webpack-dev-server 报错，是因为我们是在文件夹下搭建的本地服务器，而直接在终端上运行是在全局上运行的，所以要和上面的 build 一样来进行配置，这样配置的好处是**优先**局部的。

在package.json文件下的 scripts 中取个名字叫dev，来配置webpack-dev-server

为了能够自动打开网页，而需要我们要手动去敲服务器的地址，我们可以添加 --open

```js
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    //"dev": "webpack-dev-server"
    //为了能够自动打开网页，后面添加--open
    "dev": "webpack-dev-server --open"
    
  },
```

在终端上直接运行 npm run dev

### 配置文件的分离

将开发时需要的配置和发布时需要的配置抽离

在项目目录下新建一个build文件，里面建三个配置文件，一个基本的配置文件 base.config.js，一个开发时需要用的配置文件 dev.config.js，一个打包发布时用到的配置文件 prod.config.js。

1.安装

npm install webpack-merge@4.1.5 --save-dev

2.进行配置

base.config.js里面进行一些都需要的配置，格式和webpack.config.js相似，只是删掉了开发时需要的配置和打包发布时的配置。

 dev.config.js里面先引入 webpack-merge 和 base.config.js

```js
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig,{
    devServer: {
        contentBase: './dist',
        inline: true
    } 
})
```

prod.config.js里面引入打包时需要的插件、webpack-merge 和 base.config.js

```js
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')


module.exports = webpackMerge(baseConfig,{
    plugins: [
        new UglifyjsWebpackPlugin()
    ]
})
```

3.删除原来的配置文件，会报错，所以需要在package.json文件里的 scripts 进行修改build和dev

```js
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config ./build/prod.config.js",
    "dev": "webpack-dev-server --open --config ./build/dev.config.js"
  },
```

4.因为base.config.js里打包的路径是当前目录下，所以打包后的文件会出现在build文件夹里，所以要修改base.config.js里的出口路径

```js
output: {
        // path: 动态获取路径,在这里修改了../dist
        path: path.resolve(__dirname, '../dist'),
        filename: 'bundle.js',
        // publicPath: 'dist/'
    },
```

## CLI

Command-Line Interface，命令行界面，俗称脚手架

npm install -g @vue/cli             安装最新脚手架

**如果npm 安装脚手架出错，把这个文件夹给清空**

C:\Users\zswdaye\AppData\Roaming\npm-cache

### CLI2

拉取2.x模板

npm install -g @vue/cli-init

创建Vue CLI2初始化项目

vue init webpack 项目名称

**目录结构分析**

build文件夹和config文件夹都是webpack相关的配置

node_modules文件夹里面放着node依赖的包

static文件夹里面放静态资源

.gitignore文件忽略你不想上传服务器的东西

package.json文件里的 "scripts" 里面放着运行相关的内容。

**关闭Eslint**

如果装了ESLint，想要关掉它，可以在config文件夹的index.js文件里找到 useEslint: true, 将它改为false就可以，然后重启项目。

**runtime-compiler和runtime-only的区别**

runtime-compiler和runtime-only 从明面上看只有 main.js 文件不同。

runtime-compiler的执行流程

template-> ast ->render -> virtual dom -> UI

runtime-only的执行流程

render ->virtual dom -> UI

runtime-only性能更高，代码量更少

runtime-only和runtime-compiler中的.vue文件中的template是由vue-template-compiler解析成render函数。所以runtime-only可以以更少的代码来完成。

### CLI3

移除了build和config等配置文件；提供了vue ui命令提供可视化配置，更加人性化；移除了static文件夹，新增了public文件夹，并且index.html移动到public中。

创建Vue CLI3初始化项目

vue create 项目名称

.gitignore文件忽略你不想上传服务器的东西

#### 修改配置

第一种：启动 vue ui

​	1.导入你的项目

​	2.按照你想配置的东西添加就可以

第二种：

​	打开node_modules下的@vue文件夹，再打开cli-service文件，里面有个webpack.config.js文件，按照里面的引入，去寻找你想要的配置。

第三种：

​	在当前项目目录下建一个 vue.config.js 文件，通过 module.exports = {} ，导出你想要设置的配置，它会默认将这个配置和默认配置合并

## 路由Router

### 安装配置

一、用webpack需要安装vue-router

npm install vue-router --save

​	1.在src中创建router文件夹，并在文件夹下建一个index.html

​	2.在文件中需要配置路由相关的信息

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

//1.通过Vue.use(插件)，安装插件
Vue.use(Router)

export default new Router({
  //配置路由和组件之间的应用关系
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})
```

​	3.在main.js文件中引入router，并在Vue实例中进行挂载。引入的时候直接引入文件夹，它会自动在文件夹中找到index.html

```js
import router from './router'
new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

二、用cli直接选择vue-router就可以，它会自动帮我们完成上面的工作

### 使用vue-router的步骤

第一步：创建路由组件

```vue
<template>
  <div>
    <h2>我是首页</h2>
    <p>我是首页内容，哈哈哈</p>
  </div>
</template>
<script>
export default {
  name: 'Home'
}
</script>
<style scoped>
</style>>
```

第二步：配置路由映射：组件和路径映射关系。在router文件夹的index.html里进行配置

```js
//1.引入
import Home from '../components/Home'

//2.在routes里进行应用
routes: [
    {
      path: '/home',
      component: Home
    }
  ]
```

第三步：使用路由：通过<router-link>和<router-view>。在App.vue里进行使用

```vue
<template>
  <div id="app">
    <!-- to和上面的path对应的，当页面地址改变时，就会显示相关的页面 -->
    <router-link to="/home">首页</router-link>
    <router-view></router-view>
  </div>
</template>
```

为了不让页面一开始没有内容，给它设置一个默认组件。

```js
routes: [
    // path为空，代表这是一个默认组件，一开始就出现这里面的内容
    // path也可以写成 path: '/'  这样的效果和下面一样
    {
      path: '',
      //redirect 重定向，将它重定向到home组件
      redirect: '/home'
    }
  ],
```

页面一开始是Hash模式，地址中会出现 # ，这样不太符合我们的审美，我们可以将它改成History模式，这样就不会出现 # 。在 routes 后面添加一个 mode 的选项。

```js
mode: 'history'
```

**`<router-link>`的其他属性**

tag属性是指定将 router-link 渲染成什么标签

```vue
<router-link to="/home" tag="button">首页</router-link>
```

replace 属性将不能使用浏览器后退键

active-class:当`<router-link>`对应的路由匹配成功时，会自动给它添加一个router-link-active的class，设置active-class可以修改默认class。还有一种方法，在router文件夹的 index.html 中添加一个linkActiveClass选项。

```vue
linkActiveClass:'active'

<router-link to="/home" tag="button" replace active-class="active">首页</router-link>
```

### 路由代码跳转

通过普通的标签来进行路由切换

```vue
<button @click="homeClick">首页</button>
<button @click="aboutClick">关于</button>
methods: {
    homeClick(){
      //通过代码的方式修改路由 vue-router
      // this.$router.push('/home')
	  //replace方法不会产生历史记录，即浏览器的前进、后退没用
      this.$router.replace('/home')
      console.log("homeClick")
    },
    aboutClick(){
      // this.$router.push('/about')
      this.$router.replace('/about')
      console.log("aboutClick")
    }
  },
```

### 动态路由

1.创建路由组件

2.在router文件夹的 index.html 中引入组件，在 routes 里添加新的路由组件

```js
{
    path: '/user/:aaa',
    component: User
}
```

3.在App.vue中使用router-link来引入，为了能够获取data中的数据，使用v-bind来动态获取

```vue
<router-link :to="'/user/'+userId">用户</router-link>
<router-link :to="{path:'/user/'+userId}">用户</router-link>
data() {
    return {
      userId:'zhangsan'
    }
  },
```

4.为了能够在组件中显示userId是什么，需要在User.vue中进行配置

```vue
<h2>{{userId}}</h2>
<h2>{{$route.params.aaa}}</h2>
computed: {
      userId(){
        return this.$route.params.aaa
      }
    },
```

### 路由懒加载

懒加载方式：

一、结合Vue的异步组件和Webpack的代码分析

二、AMD写法

三、在ES6中，我们可以有更加简单的写法来组织Vue异步组件和Webpack的代码分割，也是如今我们现在要用的主要方式。以前在路由index.html直接引用的方式要放弃了，要使用路由懒加载的方式。

```js
const Home = () => import('../components/Home')
routes: [
  {
      path: '/home',
      component: Home
   }
]
---------------------------------------------------------------------
routes: [
  {
      path: '/home',
      component: () => import('../components/Home')
   }
]
```

### 路由嵌套

1.创建对应的子组件，并且在路由映射中配置对应的子路由

```js
{
	path: '/home',
	component: Home,
	children: [
	  {
		path: '',
		redirect: 'news'
	  },
	  {
		path: 'news',
		component: HomeNews
	  },
	  {
		path: 'message',
		component: HomeMessage
	  }
	]
}
```

2.在组件内部使用`<router-view>`标签，Home.vue里的template中使用

```vue
<!-- 切记不要少了/ -->
<router-link to="/home/news">新闻</router-link>
<router-link to="/home/message">消息</router-link>
<router-view></router-view>
```

### 传递参数

传递参数主要两种类型：params 和 query

params类型：

​	配置路由格式：/router/:id

​	传递的方式：在path后面跟上对应的值

​	传递后形成的路径：/router/123,  /router/abc

query的类型：  （大量参数用这个方式）

​	配置路由格式：/router, 也就是普通配置

​	传递方式：对象中使用query的key 作为传递方式

​	传递后形成的路径：/router?id=123,  /router/?id=abc

```vue
//App.vue
<router-link :to="'/user/'+userId">用户</router-link>
<router-link :to="{name:'user', params:{id:userId}}">用户</router-link>
<router-link :to="{path: '/profile', query: {name: 'why', age: 18, height: 1.88}}">档案</router-link>
//User.vue
<h2>{{$route.params.id}}</h2>
//Profile.vue
<h2>{{$route.query.name}}</h2>
<h2>{{$route.query.age}}</h2>
<h2>{{$route.query.height}}</h2>
```

```vue
<button @click="userClick">用户</button>
<button @click="profileClick">档案</button>
//在methods中定义函数
userClick(){
	//this.$router.push('/user/' + this.userId)
	this.$router.push({name:'user', params:{id:this.userId}})
},
profileClick(){
	this.$router.push({
		path: '/profile',
		query: {
			name: 'kobe',
			age: 19,
			height: 1.88
		}
	})
}
```

组件Vue中不需要更改

### 重复点击路由的报错问题

```html
方法1：在项目目录下运行 npm i vue-router@3.0 -S 重新下载未出错版本即可；

方法2：不想更换 vue-router 的版本亦可，在 main.js 或 router.js 中添加以下代码：
// 解决重复点击导航路由报错
const originalPush = VueRouter.prototype.push;
VueRouter.prototype.push = function push(location) {
 return originalPush.call(this, location).catch(err => err);
}
```

### 导航守卫

生命周期函数，用在组件.vue导出的地方

```vue
export default {
  name: 'Home',
  data() {
    return {
      message: '你好啊'
    }
  },
  //创建组件时会回调
  created() {
    console.log('created')
  },
  //挂载到整个DOM上的时候会回调
  mounted() {
    console.log('mounted')
  },
  //界面发生刷新的时候回调
  updated() {
    console.log('updated')
  },
}
```

**全局导航守卫**

​	- 主要用来监听路由的进入和离开，beforeEach和afterEach的钩子函数，它们会在路由即将改变前和改变后触发。

利用 beforeEach 来修改标题

1.在router文件夹下的index.html文件中，给每个需要标题的路由添加一个meta: 元数据(描述数据的数据)

```js
{
      path: '/about',
      component: About,
      meta:{
        title: '关于'
      },
    },
```

2.用router对象调用beforeEach函数，从一个路由跳转到另一个路由的时候就会调用这个函数。

```js
router.beforeEach((to, from, next) => {
  // 从from到to，to就是每个路由，但是调用的时候可能会出现/home/news
  // 这样就取不到title了，但to.matched[0]代表的是路由本身/home，而不是路由的子代/news
  document.title = to.matched[0].meta.title
  //一定要调用next
  next()
})
```

afterEach不需要主动调用next()函数，这些都是全局守卫

路由独享的守卫和组件内的守卫可以在Vue Router官网查看，用法和上面的差不多。

### keep-alive

keep-alive是Vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

​	它有两个属性：

​		include - 字符串或正则表达式，只有匹配的组件会被缓存

​		exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存

```vue
//这里Profile,User逗号后不能随便加空格。跟正则有关的不要随便加空格
<keep-alive exclude="Profile,User">
    <router-view/>
</keep-alive>
```

### TabBar路径引用

写路径的时候大量出现  ../../../  ，这样是不合理的，所以我们要给路径起别名。

1.在build文件夹下的 webpack.base.conf.js 文件中的 resolve 选项，从里面我们可以看到，它已经帮我们取了一个 @ 的别名，这个别名代表的是 src 文件。

比如：我们想引用 src 文件下的 main.js 文件，无论我们在哪个文件中，我们都可以这样写 @/main.js

2.为了更加方便，我们给自己取一个更方便的别名

```js
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      '@': resolve('src'),
       //这个别名代表的是 src文件下的assets文件，下面的都是相同意思
      'assets': resolve('src/assets'),
      'components': resolve('src/components'),
      'views': resolve('src/views'),
    }
  },
```

3.引用的时候，如果是 import 这样的方式引用，直接  别名/xxxx 就可以。但是，类似于html本来的属性引用就要在前面加一个波浪号  ~，如：src想要引用就要 ~别名/xxxx

```vue
import MainTabBar from 'components/MainTabBar'

<img slot="item-icon" src="~assets/img/tabbar/category.svg" alt="">
```

上面是 cli2 方式改配置文件，可以找到build文件夹。但是 cli3 没有配置文件，我们可以通过新建vue.config.js 文件，通过 module.exports = {} ,来进行配置修改。

cli3如下配置：

新建vue.config.js 文件，通过 module.exports = {configureWebpack: {}} ,来进行配置修改

```js
module.exports = {
  configureWebpack: {
    resolve: {
      alias: {
        'assets': '@/assets',
        'common': '@/common',
        'components': '@/components',
        'network': '@/network',
        'views': '@/views',
      }
    }
  }
}
```

## Promise

### 基本使用

什么情况下会用到Promise?

一般情况下是有异步操作时,使用Promise对这个异步操作进行封装

new -> 构造函数(1.保存了一些状态信息 2.执行传入的函数)

在执行传入的回调函数时, 会传入两个参数, resolve, reject.本身又是函数

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
      // 成功的时候调用resolve
      // resolve('Hello World')

      // 失败的时候调用reject
      reject('error message')
    }, 1000)
  }).then((data) => {
    console.log(data);
  }).catch((err) => {
    console.log(err);
  })
```

另外的处理方式

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
      // resolve('Hello Vuejs')
      reject('error message')
    }, 1000)
  }).then(data => {
    console.log(data);
  }, err => {
    console.log(err);
  })
```

### 链式调用

```js
new Promise((resolve, reject) => {
    // 第一次网络请求的代码
    setTimeout(() => {
      resolve()
    }, 1000)
  }).then(() => {
    // 第一次拿到结果的处理代码
    console.log('Hello World');

    return new Promise((resolve, reject) => {
      // 第二次网络请求的代码
      setTimeout(() => {
        resolve()
      }, 1000)
    })
  }).then(() => {
    // 第二次处理的代码
    console.log('Hello Vuejs');

    return new Promise((resolve, reject) => {
      // 第三次网络请求的代码
      setTimeout(() => {
        resolve()
      })
    })
  }).then(() => {
    // 第三处理的代码
    console.log('Hello Python');
  })
```

链式调用简写一(对resolve的简写)

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('aaa')
    }, 1000)
  }).then(res => {
    // 1.自己处理10行代码
    console.log(res, '第一层的10行处理代码');
    // 2.对结果进行第一次处理
    return Promise.resolve(res + '111')
  }).then(res => {
    console.log(res, '第二层的10行处理代码');

    return Promise.resolve(res + '222')
  }).then(res => {
    console.log(res, '第三层的10行处理代码');
  })
```

链式调用简写二(对resolve的简写)

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('aaa')
    }, 1000)
  }).then(res => {
    // 1.自己处理10行代码
    console.log(res, '第一层的10行处理代码');
    // 2.对结果进行第一次处理
    return res + '111'
  }).then(res => {
    console.log(res, '第二层的10行处理代码');

    return res + '222'
  }).then(res => {
    console.log(res, '第三层的10行处理代码');
  })
```

链式调用简写三(对reject的简写)

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('aaa')
    }, 1000)
  }).then(res => {
    // 1.自己处理10行代码
    console.log(res, '第一层的10行处理代码');
    // 2.对结果进行第一次处理
    // return Promise.reject('error message')
    throw 'error message'
  }).then(res => {
    console.log(res, '第二层的10行处理代码');

    return Promise.resolve(res + '222')
  }).then(res => {
    console.log(res, '第三层的10行处理代码');
  }).catch(err => {
    console.log(err);
  })
```

因为错误只会出现一次，所以只有一次.catch()

### all方法

```js
Promise.all([
      // new Promise((resolve, reject) => {
      //   $.ajax({
      //     url: 'url1',
      //     success: function (data) {
      //       resolve(data)
      //     }
      //   })
      // }),
      // new Promise((resolve, reject) => {
      //   $.ajax({
      //     url: 'url2',
      //     success: function (data) {
      //       resolve(data)
      //     }
      //   })
      // })

    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({name: 'why', age: 18})
      }, 2000)
    }),
    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({name: 'kobe', age: 19})
      }, 1000)
    })
  ]).then(results => {
    console.log(results);
  })
```

## Vuex

多个界面需要共享的数据放在vuex

### 安装、配置

npm install vuex --save

1.在src文件夹下新建一个文件夹 store ，在新建的文件夹下新建一个 index.js 文件

2.在 index.js 文件中，引入Vue 和Vuex，安装Vuex插件，创建Store对象，导出对象

```js
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)

//2.创建对象
const store = new Vuex.Store({
  
})

//3.导出store
export default store
```

3.在main.js中引入store，在vue实例中进行挂载

```js
import store from './store'
new Vue({
  el: '#app',
  store,
  render: h => h(App)
})
```

### Vuex使用

在store文件夹下的 index.js 文件的 store对象中有几个选项

state、mutations、actions、getters、modules

state中存放数据或共享状态，在其他组件中要使用数据的时候，$store.state.xxx，不推荐直接这样修改数据。

```js
state: {
    counter: 100
  },
<h2>{{$store.state.counter}}</h2>  
```

利用mutations修改数据，一般是同步操作。

​	在mutations中定义方法，进行数据的更改

```js
mutations: {
    // 方法
    increment(state){
      state.counter++
    },
    decrement(state){
      state.counter--
    }
  },
```

​	在想要修改数据的组件中，使用 $store.commit('mutations中方法') 来进行修改数据

```vue
<button @click="addition">+</button>
<button @click="subtraction">-</button>
methods: {
    addition(){
      this.$store.commit('increment')
    },
    subtraction(){
      this.$store.commit('decrement')
    }
  },
```

#### state

state单一状态树

​	- 只使用一个store对象，在之后的数据维护很方便

#### getters基本使用

类似于vue中的计算属性。

想要获取 state中数据变化后的数据，调用getters

```vue
state: {
    counter: 100,
    students: [
      {id: 110, name: 'why', age: 18},
      {id: 111, name: 'kobe', age: 25},
      {id: 112, name: 'james', age: 26},
      {id: 113, name: 'curry', age: 15},
    ]
  },
  getters: {
    powerCounter(state){
      return state.counter*state.counter
    },
    more20stu(state){
      return state.students.filter( s => s.age>20)
    }
  },
  
//组件中使用
<h2>{{$store.getters.powerCounter}}</h2>
<h2>{{$store.getters.more20stu}}</h2>
```

**传递参数**，在 getters 的方法中传递参数，第一个永远是state，第二个是getters。第二个名字可以换，但它永远指代的是getters

```js
getters: {
    powerCounter(state){
      return state.counter*state.counter
    },
    more20stu(state){
      return state.students.filter( s => s.age>20)
    },
    // 这里的参数getters，写成aaa都可以，但它指代的永远都是getters
    more20stuLength(state,getters){
      //可以使用 getters 里的方法
      return getters.more20stu.length
    },
    moreAgeStu(state){
      // 想要传递其他参数，只能通过返回一个函数，在函数里传递其他参数
      // return function (age){
      //   return state.students.filter( s => s.age>age)
      // }
      return age => {
        return state.students.filter( s => s.age>age)
      }
    }
  },
//组件中使用
<h2>{{$store.getters.powerCounter}}</h2>
<h2>{{$store.getters.more20stu}}</h2>
<h2>{{$store.getters.more20stuLength}}</h2>
// 传递了其他参数 如：15
<h2>{{$store.getters.moreAgeStu(15)}}</h2>
```

#### mutations

Vuex要求我们Mutation中的方法必须是同步方法。

先在mutations中定义方法，确定需要进行的操作；然后在组件中定义一个方法，通过 this.$store.commit('mutations中的方法', 参数) 来使用这个方法

```vue
mutations: {
    // 方法
    increment(state){
      state.counter++
    },
    decrement(state){
      state.counter--
    },
    incrementCount(state, count){
      state.counter += count
    }
  },
//组件中定义方法
methods: {
    addition(){
      this.$store.commit('increment')
    },
    subtraction(){
      this.$store.commit('decrement')
    },
    addCount(count){
      this.$store.commit('incrementCount', count)
    }
  },
//组件进行使用
<button @click="addition">+</button>
<button @click="subtraction">-</button>
<button @click="addCount(5)">+5</button>
<button @click="addCount(10)">+10</button>
```

**mutation提交风格**

```js
//1.普通的提交风格
mutations: {
	incrementCount(state, count){
      state.counter += count  
    },
}
//组件的methods中定义的方法
addCount(count){
	this.$store.commit('incrementCount', count)
//2.特殊的提交风格
this.$store.commit({
type: 'incrementCount',
count
})
},
//2.特殊的提交风格，它会将commit里的对象给传过去
mutations: {
	incrementCount(state, payload){
      state.counter += payload.count  
    },
}
//组件的methods中定义的方法
addCount(count){
    this.$store.commit({
    	type: 'incrementCount',
    	//count: count
        count
    })
},
```

**mutation响应规则**

提前在store中初始化好所需的属性

当给state中的对象添加新属性时，使用Vue.set(obj, 'newProp', 123)

```vue
Vue.set(state.info, 'address', '洛杉矶')
```

当给state中的对象删除新属性时，使用 Vue.delete(obj, '属性')

```vue
Vue.delete(state.info, 'age')
```

**mutation常量规则**

方法过多，需要不断切换文件来查看方法名是否写错，为解决这一问题，使用了mutation常量规则。

1.在store文件夹中新建一个mutations-types.js文件，在里面导出你所命名的方法名

```js
export const INCREMENT = 'increment'
```

2.在store 文件夹的index.js中引入上面的文件，在mutations里进行使用

```js
import {INCREMENT} from './mutations-types'
mutations: {
    // increment(state){
    //   state.counter++
    // },
    [INCREMENT](state){
      state.counter++
    },
}
```

3.在组件的methods中使用，先引用文件

```vue
import {INCREMENT} from './store/mutations-types'
methods: {
    addition(){
      this.$store.commit(INCREMENT)
    },
}
```

#### actions

Action类似于Mutation, 但是是用来代替Mutation进行异步操作的。

三种使用方式：

1.不带参数

```js
//index.js
actions: {
    aUpdateInfo(context){
        //这里进行异步操作，'updateInfo'是mutations上的方法名,完成异步操作交给mutations
        setTimeout(() =>{
			context.commit('updateInfo')
        },1000)
    }
}
//组件中
methods: {
	updateInfo(){
    	this.$store.dispatch('aUpdateInfo')
    }
}
```

2.带参数

```js
//index.js
actions: {
    aUpdateInfo(context, payload){
       setTimeout(() =>{
         context.commit('updateInfo')
         console.log(payload.message);
         payload.success()
       },1000)
    }
}
//组件中
methods: {
	updateInfo(){
    	this.$store.dispatch('aUpdateInfo',{
          message: '我是携带的信息',
          success: () =>{
            console.log('里面已经完成了');
          }
        })
    }
}
```

3.带参数优化后

```js
//index.js
actions: {
    aUpdateInfo(context, payload){
       return new Promise((resolve,reject) => {
         setTimeout(() =>{
           context.commit('updateInfo')
           console.log(payload);

           resolve('11111')
         },1000)
       })
    }
}
//组件中
methods: {
	updateInfo(){
      this.$store
        .dispatch('aUpdateInfo','我是携带的信息')
        .then(res => {
          console.log('里面完成了提交');
          console.log(res);
      })
    }
}
```

#### modules

modules是为了防止state中的数据太多太杂，store对象就有可能变得相当臃肿，不容易维护，而创造出来的模块。Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutations、actions、getters等。

```js
modules: {
	a: moduleA
}
const moduleA = {
	state: {},
	mutations: {},
	getters: {},
	actions: {}
}
```

##### state

模块的state里的数据。其实是将模块包装成对象，state里的数据就是对象的属性。

```js
// index.js中的a模块
state: {
   name: 'zhangsan'
},
// 组件中进行使用
<h2>{{$store.state.a.name}}</h2>
```

##### mutations

和根mutations一样，最好不要取和根mutations里的方法一样的名字，它会先查看根mutations再查看模块里的。

```js
// index.js中的a模块
mutations: {
    updateName(state, payload){
      state.name = payload
    }
  },
// 组件中进行使用
methods: {
	updateName(){
      this.$store.commit('updateName', 'lisi')
    },
}
```

##### getters

既能调用本身的state，又能调用根state(rootState)

```js
// index.js中的a模块
getters: {
    fullname(state){
      return state.name + '1111'
    },
    fullname2(state, getters){
      return getters.fullname + '2222'
    },
    fullname3(state, getters, rootState){
      return getters.fullname2 + rootState.counter
    }
  },
// 组件中进行使用
<h2>{{$store.getters.fullname}}</h2>
<h2>{{$store.getters.fullname2}}</h2>
<h2>{{$store.getters.fullname3}}</h2>
```

##### actions

```js
// index.js中的a模块
actions: {
    aUpdateName(context){
      //context.commit 只会调用自己模块里的mutations的方法
      setTimeout(() =>{
        context.commit('updateName','wangwu')
        // 通过打印可以发现context也能调用根getters，根state等。
        console.log(context);
      }, 1000)
    }
  }
// 组件中进行使用
asyncUpdatename(){
   this.$store.dispatch('aUpdateName')
}
```

**除了state的调用和根上的state不一样，其余的都相同**

## 网络请求axios

安装

npm install axios --save

### 使用

1.先引入axios

import axios from 'axios'

2.简单使用 axios(config)

axios默认是get请求

```js
axios({
  url: 'http://123.207.32.32:8000/home/data',
  //这里可以选择请求方式
  method: 'get'
  //专门针对get请求的参数拼接
  params: {
    type: 'pop',
    page: 1
  }
 //从then中获取数据 res
}).then(res => {
  console.log(res);
})
```

3.axios发送并发请求

也就是同时拿到两个请求的数据

```js
axios.all([axios({
   url: 'http://123.207.32.32:8000/home/multidata'
 }), axios({
   url: 'http://123.207.32.32:8000/home/data',
   params: {
     type: 'sell',
     page: 5
   }
 })]).then(results => {
     console.log(results);
   })
//也可以将两个请求的数据单独显示
axios.all([axios({
  url: 'http://123.207.32.32:8000/home/multidata'
}), axios({
  url: 'http://123.207.32.32:8000/home/data',
  params: {
    type: 'sell',
    page: 5
  }
})]).then(axios.spread((res1, res2) => {
  console.log(res1);
  console.log(res2);
}))
```

### 配置

全局配置

使用过后就不需要在url中传太长的重复的东西。

axios.defaults.baseURL = 'http://152.136.185.210:8000/api/z8'

axios.defaults.timeout = 5000

```js
axios.defaults.baseURL = 'http://152.136.185.210:8000/api/z8'
axios.defaults.timeout = 5000

axios.all([axios({
  url: '/home/multidata'
}), axios({
  url: '/home/data',
  params: {
    type: 'sell',
    page: 5
  }
})]).then(axios.spread((res1, res2) => {
  console.log(res1);
  console.log(res2);
}))
```

### 实例

真实开发过程中，会对多个服务器来进行请求访问，使用全局的URL就不是很合理了。

```js
const instance1 = axios.create({
  baseURL: 'http://152.136.185.210:8000/api/z8',
  timeout: 5000
})

instance1({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
})

instance1({
  url: '/home/data',
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res);
})
```

### 封装

如果在想要进行网络请求的组件中进行引入axios，然后使用，在后期维护修改的时候，会造成很大的困扰，需要不断的更改每个文件。所以才有了封装。

1.创建文件夹network，里面创建一个request.js的文件。

不使用 export default 的方式导出，是为了以后能够导出更多的实例

```js
//request.js
import axios from 'axios'
export function request(config, success, failure) {
  //1.创建axios的实例
  const instance = axios.create({
    baseURL: 'http://152.136.185.210:8000/api/z8',
    timeout: 5000
  })

  // 发送真正的网络请求
  instance(config)
    .then(res => {
      success(res)
    })
    .catch(err => {
      failure(err)
    })
}
//其他地方进行使用
import {request} from './network/request'
request({
  url: '/home/multidata'
}, res => {
  console.log(res);
}, err => {
  console.log(err);
})
```

2.第二种简便封装

```js
//request.js
 export function request(config) {
   //1.创建axios的实例
   const instance = axios.create({
     baseURL: 'http://152.136.185.210:8000/api/z8',
     timeout: 5000
   })
   // 发送真正的网络请求
   instance(config.baseConfig)
     .then(res => {
       config.success(res)
     })
     .catch(err => {
       config.failure(err)
     })
 }
//其他地方进行使用
import {request} from './network/request'
 request({
   baseConfig: {
     url: '/home/multidata'
   },
   success: function (res) {
     console.log(res);
   },
   failure: function (err) {
     console.log(err);
   }
 })
```

3.第三种简便封装

```js
//request.js
 export function request(config) {
   return new Promise((resolve,reject) => {
     //1.创建axios的实例
     const instance = axios.create({
       baseURL: 'http://152.136.185.210:8000/api/z8',
       timeout: 5000
     })
     // 发送真正的网络请求
     instance(config)
       .then(res => {
         resolve(res)
       })
       .catch(err => {
         reject(err)
       })
   })
 }
//其他地方进行使用
import {request} from './network/request'
request({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

4.第四种简便封装(最终封装方案)

因为 axios.create 创建出来的实例本身就是Promise

```js
//request.js
export function request(config) {
    //1.创建axios的实例
    const instance = axios.create({
      baseURL: 'http://152.136.185.210:8000/api/z8',
      timeout: 5000
    })

    // 发送真正的网络请求
    return instance(config)     
}
//其他地方进行使用
import {request} from './network/request'
request({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

### 拦截器

axios提供了拦截器，用于我们在发送每次请求或者得到相应后，进行对应的处理；一般是在创建axios的地方进行处理，在引入的地方还是照常使用。

axios.interceptors是全局的拦截，一般都是使用具体实例的拦截。

```js
//request.js
export function request(config) {
    // 1.创建axios的实例
    const instance = axios.create({
      baseURL: 'http://152.136.185.210:8000/api/z8',
      timeout: 5000
    })
    // 2.axios的拦截器
    // 2.1请求拦截的作用
    instance.interceptors.request.use(config => {
      // console.log(config);
      // 1.比如config中的一些信息不符合服务器的要求
      // 2.比如每次发送网络请求时，都希望在界面中显示一个请求的图标
      // 3.某些网络请求(比如登录(token))，必须携带一些特殊的信息
      //每次拦截成功后记得返回
      return config
    }, err => {
      // console.log(err);
    })
    //响应拦截
    instance.interceptors.response.use(res => {
      // console.log(res);
      //其实我们需要的是结果中的data数据，所以可以直接返回res.data
      return res.data
    }, err => {
      console.log(err);
    })
    // 3.发送真正的网络请求
    return instance(config)     
}
//其他地方进行使用
import {request} from './network/request'
request({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

