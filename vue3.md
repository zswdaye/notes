# VUE3

## 组合式API

### setup

setup选项是一个接收 `props` 和 `context` 的函数 ，在这函数里面不能访问data、computed、methods、refs(模板ref)等，因为执行setup的时候组件还没有被创建出来

> props是响应式的，不能使用ES6解构，它会消除prop的响应性 ，如果要解构就要使用toRefs函数来完成。context不是响应式的可以使用ES6解构。

context是一个普通的js对象，其中含有attrs、slots、emit、expose等

```js
import { toRefs } from 'vue'
export default {
	props: {
		user: {
			type: String,
      		required: true
		}
    },
	setup(props, context) {
		console.log(props) // { user: '' }
        const { user } = toRefs(props)
        // Attribute (非响应式对象，等同于 $attrs)
        console.log(context.attrs)
        // 插槽 (非响应式对象，等同于 $slots)
        console.log(context.slots)
        // 触发事件 (方法，等同于 $emit)
        console.log(context.emit)
        // 暴露公共 property (函数)
        console.log(context.expose)
    }
}
```

### ref、reactive、toRef、toRefs

```js
import { ref, reactive, toRef, toRefs } from 'vue'
/* ref一般用于定义基本数据类型，因为数据量少，方便使用，它会为我们的值创建一个响应式引用，获取我们所需要的值一般是.value的形式，因为它会将值包裹在对象里
*/
const counter = ref(0)
console.log(counter) // { value: 0 }
console.log(counter.value) // 0
/*reactive一般用于定义引用数据类型，因为数据量大，能够更好的处理，它也会为我们的值创建一个响应式引用，它里面包含的是一个对象，有点类似于data选项return出来的对象。
	它使用不需要像ref一样需要.value获取值，直接.属性就能获取到值
*/
const data = reactive({title: 'Vue3真好用'})
console.log(data.title) // 'Vue3真好用'
// toRef和toRefs都是将响应式对象转换为普通对象，其中结果对象的每个property都是指向原始对象相应property的ref, toRef只能转换单个，toRefs可以转换多个
const state = reactive({
  foo: 1,
  bar: 2
})
const fooRef = toRef(state, 'foo')
fooRef.value++
console.log(state.foo) // 2
state.foo++
console.log(fooRef.value) // 3
---------------------------------------------
const stateAsRefs = toRefs(state)
state.foo++
console.log(stateAsRefs.foo.value) // 2
stateAsRefs.foo.value++
console.log(state.foo) // 3
```

> **如果将reactive定义的对象进行解构的话，会将其响应性丢失。但是使用toRefs后进行解构的话不会丢失它的响应性**

### 生命周期钩子

选项式API中有以下钩子函数`beforeCreate`、`created`、`beforeMount`、`mounted`、`beforeUpdate`、`updated`、`beforeUnmount`、`unmounted`、`errorCaptured`、`renderTracked`、`renderTriggered`、`activated`、`deactivated` 一共13种

在setup()里可以通过on+生命周期钩子来访问组件的生命周期钩子

> 注意：这里没有beforeCreate和created的生命周期钩子，因为setup是围绕 `beforeCreate` 和 `created` 生命周期钩子运行的，它运行在它们前面

```js
onBeforeMount(()=>console.log('Component is beforeMount!'))
onMounted(()=>console.log('Component is mounted!'))
onBeforeUpdate(()=>console.log('Component is beforeUpdate	!'))
onUpdated(()=>console.log('Component is updated!'))
//这里对应的是vue2中的生命周期
onBeforeUnmount(()=>console.log('Component is beforeDestroy!'))
onUnmounted(()=>console.log('Component is destroyed!'))
...
```

### Provide/Inject

主要实现 父 - 后代 组件传值，以往都是通过props、emit来进行父子组件传值，这个可以实现父组件和后代组件的传值

> 这两个都只能在当前活动实例的 `setup()` 期间调用

```js
/*provide函数有两个参数
1.name (<String> 类型)
2.value
*/
import { provide } from 'vue'
setup() {
    provide('location', 'North Pole')
    provide('geolocation', {
        longitude: 90,
        latitude: 135
    })
}
/*inject函数有两个参数
1.要 inject 的 property 的 name
2.默认值 (可选)
*/
import { inject } from 'vue'
setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')
    return {
        userLocation,
        userGeolocation
    }
}
```

增加 provide 值和 inject 值之间的响应性，给provide使用ref或reactive

```js
import { provide, reactive, ref } from 'vue'
setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
        longitude: 90,
        latitude: 135
    })
    provide('location', location)
    provide('geolocation', geolocation)
}
// 这两个 property 中有任何更改，后代组件也将自动更新
// 当使用响应式 provide/inject 值时，建议尽可能将对响应式 property 的所有修改限制在定义 provide 的组件内部
// 如果要确保通过 provide 传递的数据不会被 inject 的组件更改，建议对提供者的 property 使用 readonly
provide('location', readonly(location))
```

## computed、watch

`computed`它可以接受一个getter函数或者一个具有get和set函数的对象

```js
import {computed} from 'vue'

const count = ref(1)
const plusOne = computed(() => count.value + 1)
console.log(plusOne.value) // 2
plusOne.value++ // 错误
-------------------- 分割线 ----------------------
const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: val => {
    count.value = val - 1
  }
})
plusOne.value = 1
console.log(count.value) // 0
```

`watchEffect`是一个立即执行传入的函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。

```js
import {watchEffect} from 'vue'

const count = ref(0)
watchEffect(() => console.log(count.value))
// -> logs 0
setTimeout(() => {
  count.value++
  // -> logs 1
}, 100)
```

> **想要停止侦听器，可以在需要停止的地方再次调用侦听方法就能实现停止侦听器的功能**

`watch`需要侦听特定的数据源，并在回调函数中执行副作用。默认情况下，它也是惰性的，即只有当被侦听的源发生变化时才执行回调。

侦听单个数据源

watch侦听ref数据和reactive数据形式是不一样的

```js
import {watch, ref, reactive} from 'vue'

// 直接侦听ref
const count = ref(0)
watch(count, (count, prevCount) => {
  /* ... */
})
// 侦听reactive
const state = reactive({ count: 0 })
watch(
  () => state.count,
  (count, prevCount) => {
    /* ... */
  }
)
```

侦听多个数据源

```js
import {watch, ref, reactive} from 'vue'

const firstName = ref('')
const lastName = ref('')
watch([firstName, lastName], (newValues, prevValues) => {
  console.log(newValues, prevValues)
})
firstName.value = 'John' // logs: ["John", ""] ["", ""]
lastName.value = 'Smith' // logs: ["John", "Smith"] ["John", ""]

const numbers = reactive([1, 2, 3, 4])
watch(
  () => [...numbers],
  (numbers, prevNumbers) => {
    console.log(numbers, prevNumbers)
  }
)
numbers.push(5) // logs: [1,2,3,4,5] [1,2,3,4]

const state = reactive({ 
  id: 1,
  attributes: { 
    name: '',
  }
})
watch(
  [() => state.id, () => state.attributes.name],
  ([nid,nname], [oid,oname]) => {
    console.log('id', nid, oid)
    console.log('name', nname, oname)
  }
)
// 使用deep来侦听深度嵌套对象
watch(
  () => state,
  (state, prevState) => {
    console.log('deep', state.attributes.name, prevState.attributes.name)
  },
  { deep: true }
)
state.attributes.name = 'Alex' // 日志: "deep" "Alex" "Alex"
// 因为侦听的是响应式对象或数组将始终返回该对象的当前值和上一个状态值的引用，所以要对值进行深拷贝，使用_.cloneDeep来完成
import _ from 'lodash'

watch(
  () => _.cloneDeep(state),
  (state, prevState) => {
    console.log(state.attributes.name, prevState.attributes.name)
  }
)

state.attributes.name = 'Alex' // 日志: "Alex" ""
```

## 插槽

### 默认插槽

1.子组件中定义插槽

```html
<button class="btn-primary">
  <slot></slot>
</button>
```

2.父组件中使用, 如果子组件没有定义插槽, 那么在子组件内部的所有东西都会被抛弃

```html
<my-button>
  Create a button{{item}}
</my-button>
```

3.备用内容, 在子组件的slot标签里编写备用内容(默认内容), 如果父组件没有提供内容, 那么备用内容将会被渲染出来, 提供了就会渲染父组件中的内容

```html
<button class="btn-primary">
  <slot>我是备用内容</slot>
</button>
```

在插槽中使用的数据是父组件中的data, 想要在插槽中使用子组件的数据, 在[作用域插槽](#作用域插槽)中会实现

### 具名插槽

1.子组件定义多个插槽, 以便在不同的场景下渲染不同的内容

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

2.父组件中使用插槽, 没有`name`定义的`slot`默认自带default名字

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>
  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>
  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

> 每个`v-slot`必须在`<template>`元素中, 除了被提供的内容只有默认插槽时，组件的标签才可以被当作插槽的模板来使用.

```html
<base-layout v-slot:default>
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</base-layout>
<!-- 另一种样子 -->
<base-layout>
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</base-layout>
```

#### 动态插槽名

将插槽的名字根据需要来进行动态设置，以达到渲染对应内容

```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

#### 具名插槽缩写

之前`v-slot:`的写法有些繁琐，可以将其替换成 `#`

```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>
  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>
  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
<!-- 在作用域插槽中也可以进行缩写 -->
<todo-list #default="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

### 作用域插槽

想要在父组件中使用子组件的数据来进行内容自定义

1.子组件中将自己需要的attribute绑定到`slot`中去

```html
<div>
    <ul>
      <li v-for="( item, index ) in items">
        <slot :item="item" :index="index" :another-attribute="anotherAttribute"></slot>
      </li>
    </ul>
</div>
```

2.在父组件的内容中使用它们

```html
<todo-list>
  <template v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </template>
</todo-list>
<!-- 另一种写法 -->
<todo-list v-slot:default="slotProps">
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</todo-list>
<!-- 第二种写法 -->
<todo-list v-slot="slotProps">
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</todo-list>
```

第二种写法要慎重使用，因为这是默认插槽的缩写语法，不能和具名插槽混合使用，当有具名插槽使用时一定要将默认插槽写完整`v-slot:default`, 而且还要将其写在`<template>`元素中

#### 解构插槽Prop

将作用域中的插槽数据解构出来

```html
<todo-list v-slot="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
<!-- prop重命名 -->
<todo-list v-slot="{ item: todo }">
  <i class="fas fa-check"></i>
  <span class="green">{{ todo }}</span>
</todo-list>
<!-- 定义备用内容，当插槽prop是undefined的时候 -->
<todo-list v-slot="{ item = 'Placeholder' }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

## Vuex

### 安装、配置

```
npm install vuex@next --save
```

1.在src文件夹下新建一个文件夹`store`，在新建文件夹下新建一个`index.js`文件

2.在`index.js`文件中引入`vuex`，创建`store`实例，导出实例

```js
import { createStore } from 'vuex'
// 创建一个新的 store 实例
const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
// 导出实例
export default store
```

3.在`main.js`中引入store，并使用它

```js
import { createApp } from 'vue'
import store from './store'

createApp(App).use(store).mount('#app')
```

### vuex的基本使用

在组件中通过引入，调用`useStore`函数，来访问store

```js
import { useStore } from 'vuex'

setup () {
  const store = useStore()
  // 这个store等效于 this.$store
}
```

访问`state`和`getter`需要创建`computed`引用以保留响应性

```js
import { computed } from 'vue'
import { useStore } from 'vuex'

setup () {
  const store = useStore()
  // 在 computed 函数中访问 state
  const count = computed(() => store.state.count),
  // 在 computed 函数中访问 getter
  const double = computed(() => store.getters.double)
}
```

访问`mutation`和`action`只需要调用`commit`和`dispatch`函数即可

```js
import { useStore } from 'vuex'

setup () {
  const store = useStore()
  // 使用 mutation
  const increment = () => store.commit('increment'),
  // 使用 action
  const asyncIncrement = () => store.dispatch('asyncIncrement')
}
```

## vite构建工具

### process的使用

在vite中无法使用process.env了，要想使用process.env有两种方式

```js
1.使用import.meta.env来代替process.env
2.在vite.config.js中定义
export default defineConfig({
    ...
    define: {
    'process.env': {}
  }
})
```

.env配置文件书写

> 为了防止意外地将一些环境变量泄漏到客户端，只有以 `VITE_` 为前缀的变量才会暴露给经过 vite 处理的代码

**.env.development文件**

```log
ENV='development'
NODE_ENV='development'
VITE_SOME_KEY=45874
VITE_OK_DE=123
```

**.env.production文件**

```log
ENV='production'
NODE_ENV='production'
VITE_SOME_KEY=45874
VITE_OK_DE=123
```

只有`VITE_SOME_KEY`和`VITE_OK_DE`能够被`import.meta.env` . 出来, 而`process.env` . 不出任何东西, 但可以使用`process.env.NODE_ENV`来获取当前是什么生产环境

### jsx的使用

1.需要下载`@vitejs/plugin-vue-jsx`插件

```js
npm install -D @vitejs/plugin-vue-jsx
```

2.导入插件, 在vite.config.js中进行配置

```js
// vite.config.js
import vueJsx from '@vitejs/plugin-vue-jsx'
export default defineConfig({
    plugins: [vueJsx({})]
})
```

3.新建 *.jsx 文件, 编写jsx文件

### css modules的使用

在jsx中直接引入css文件, 这些css文件中的样式都是全局的, 如果项目过大, 可能会出现命名相同, 导致样式错乱, 所以要使用css modules将这些样式进行模块化封装.

1.在vite.config.js文件中进行配置

```js
// vite.config.js
export default defineConfig({
    css: {
        //* css模块化
        modules: { // css模块化 文件以.module.[css|less|scss]结尾
          generateScopedName: '[name]--[local]---[hash:base64:5]',
          hashPrefix: 'prefix',
        },
      }
})
```

2.新建 *.module.css 文件, 名字一定要以 .module.[css|less|scss]结尾, 在文件里书写样式, 以标签名书写的样式不会进行模块封装, 所以最好都加上class或id等属性名

3.在jsx或者其他*.vue中引用

```js
import styles from '../styles/todolist.module.scss'
// *.jsx
<p className={styles.itemText}>{item.value}</p>
<button type="primary" class={styles.button} onClick={this.addUser}>新增</button>
```

## vue2到vue3变化

### v-model

1.在自定义组件时，`v-model` prop 和事件默认名称已更改：

- prop：`value` -> `modelValue`；
- 事件：`input` -> `update:modelValue`；

```js
// vue2
<ChildComponent v-model="pageTitle" />
<!-- 是以下的简写: -->
<ChildComponent :value="pageTitle" @input="pageTitle = $event" />
/* 改变prop或事件名称 */
<ChildComponent v-model="pageTitle" />
// ChildComponent.vue
export default {
  model: {
    prop: 'title',
    event: 'change'
  },
  props: {
    // 这将允许 `value` 属性用于其他用途
    value: String,
    // 使用 `title` 代替 `value` 作为 model 的 prop
    title: {
      type: String,
      default: 'Default title'
    }
  }
}
<!-- 是以下的简写: -->
<ChildComponent :title="pageTitle" @change="pageTitle = $event" />
// vue3
<ChildComponent v-model="pageTitle" />
<!-- 是以下的简写: -->
<ChildComponent :modelValue="pageTitle" @update:modelValue="pageTitle = $event" />
/* 更改 model 的名称 */
<ChildComponent v-model:title="pageTitle" />
<!-- 是以下的简写: -->
<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
```

2.`v-bind` 的 `.sync` 修饰符和组件的 `model` 选项已移除，可在 `v-model` 上加一个参数代替；

3.可以在同一个组件上使用多个 `v-model` 绑定

```js
// vue2
/* 在某些情况下，我们可能需要对某一个 prop 进行“双向绑定”，建议使用 update:myPropName 抛出事件 */
<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
// 为了方便起见，可以使用 .sync 修饰符来缩写
<ChildComponent :title.sync="pageTitle" />
// vue3
/* 作为 .sync 修饰符的替代，在自定义组件上可以使用多个 v-model */
<ChildComponent v-model:title="pageTitle" v-model:content="pageContent" />
<!-- 是以下的简写： -->
<ChildComponent
  :title="pageTitle"
  @update:title="pageTitle = $event"
  :content="pageContent"
  @update:content="pageContent = $event"
/>
```

3.可以自定义`v-model` 修饰符

除了像 `.trim` 这样的 2.x 硬编码的 `v-model` 修饰符外，现在 3.x 还支持自定义修饰符：

```vue
// 添加到组件 v-model 的修饰符将通过 modelModifiers prop提供给组件
<ChildComponent v-model.capitalize="pageTitle" />
// ChildComponent.vue
<template>
	<input type="text" :value="modelValue" @input="emitValue">
</template>
<script>
export default{
    props:{
        modelValue: String,
        modelModifiers: {
          default: () => ({})
        }
    },
    emits: ['update:modelValue'],
    methods: {
      emitValue(e) {
        let value = e.target.value
        if (this.modelModifiers.capitalize) {
          value = value.charAt(0).toUpperCase() + value.slice(1)
        }
        this.$emit('update:modelValue', value)
      }
    },
}
</script>
```

对于带参数的 v-model 绑定，生成的 prop 名称将为 arg + "Modifiers"：

```js
<my-component v-model:description.capitalize="myText"></my-component>
// my-component.vue
props: ['description', 'descriptionModifiers'],
```

