## Vue

###  

#### 1 什么是 vue

vue 是一套用于构建用户界面的前端框架（框架是一套现成的解决方案）。

#### 2 Vue 的特性

vue 框架的特性，主要体现在如下两方面：

① 数据驱动视图

在使用了 vue 的页面中，vue 会监听数据的变化，从而自动重新渲染页面的结构。

好处：当页面数据发生变化时，页面会自动重新渲染！

主要：数据驱动视图是单向的数据绑定。

② 双向数据绑定

在填写表单时，双向数据绑定可以辅助开发者在不操作 DOM 的前提下，自动把用户填写的内容同步到数据源中。

好处：开发者不再需要手动操作 DOM 元素，来获取表单元素最新的值。

#### 3 MVVM

MVVM 是 vue 实现数据驱动视图和双向数据绑定的核心原理。MVVM 指的是 Model、View 和 ViewModel，它把每个 HTML 页面都拆分成了这三个部分。

在 MVVM 概念中：

- Model 表示当前页面渲染时所依赖的数据源。
- View 表示当前页面所渲染的 DOM 结构。
- ViewModel 表示 vue 的实例，它是 MVVM 的核心。

#### 4 基本使用步骤

① 导入 vue.js 的 script 脚本文件

② 在页面中声明一个将要被 vue 所控制的 DOM 区域

③ 创建 vm 实例对象（vue 实例对象）

```html
<body>
    <!-- 2. 在页面中声明一个将要被 vue 所控制的 DOM 区域 -->
    <div id="app">{{ username }}</div>
    
    <!-- 1. 导入 vue.js 的 script 脚本文件 -->
 	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
    <script>
        // 3. 创建 vm 实例对象（vue 实例对象）
        const vm = new Vue({
            // 3.1 指定当前 vm 实例要控制页面的哪个区域
        	el: '#app',
        	// 3.2 指定 Model 数据源
        	data: {
            	username: 'zs'
        	}
        })
    </script>
</body>
```

#### 5 指令的概念

指令（Directives）是 vue 为开发者提供的模板语法，用于辅助开发者渲染页面的基本结构。

vue 中的指令按照不同的用途可以分为如下 6 大类：

##### ① 内容渲染指令

内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有如下 3 个：

- v-text

  用法示例：

  ```html
  <!-- 把 username 对应的值，渲染到第一个 p 标签中 -->
  <p v-text="username"></p>
  
  <!-- 把 gender 对应的值，渲染到第二个 p 标签中 -->
  <!-- 注意：第二个 p 标签中，默认的文本“性别”会被 gender 的值覆盖掉 -->
  <p v-text="gender">性别</p>
  ```

  注意：v-text 指令会覆盖元素内默认的值。

- {{ }}

  vue 提供的 {{ }} 语法，专门用来解决 v-text 会覆盖默认文本内容的问题。这种 {{ }} 语法的专业名称是插值表达式（英文名为：Mustache）。

  用法示例：

  ```html
  <!-- 使用 {{ }} 插值表达式，将对应的值渲染到元素的内容节点中， -->
  <!-- 同时保留元素自身的默认值 -->
  <p>姓名：{{ username }}</p>
  <p>性别：{{ gender }}</p>
  ```

  注意：

  1. 相对于 v-text 指令来说，插值表达式在开发中更常用一些！因为它不会覆盖元素中默认的文本内容。
  2. 插值表达式只能用在元素的内容节点中，不能用在元素的属性节点中！

- v-html

  v-text 指令和插值表达式只能渲染纯文本内容。如果要把包含 HTML 标签的字符串渲染为页面的 HTML 元素，则需要用到 v-html 这个指令：

  ```html
  <!-- 假设 data 中定义了名为 description 的数据，数据的值为包含 HTML 标签的字符串： -->
  <!-- '<h5 style="color: red;">我在黑马程序员学习 vue.js 课程。</h5>' -->
  
  <p v-html="description"></p>
  ```

  

##### ② 属性绑定指令

如果需要为元素的属性动态绑定属性值，则需要用到 v-bind 属性绑定指令。用法示例如下：

```html
<input type="text" v-bind:placeholder="tips">

<script>
    ...
    // 数据源
    data: {
		tips: '请输入用户名'
	}
    ...
</script>
```

##### v-bind 简写形式：`:`

```html
<input type="text" :placeholder="tips">
```

##### ③ 事件绑定指令

vue 提供了 v-on 事件绑定指令，用来辅助程序员为 DOM 元素绑定事件监听。语法格式如下：

```html
<h3>count 的值为：{{ count }}</h3>
<!-- 语法格式为 v-on:事件名称="事件处理函数的名称" -->
<button v-on:click="addCount">+1</button>

<!-- 绑定事件处理函数并传参 -->
<button v-on:click="addCount(n)">+n</button>
```

###### v-on 简写形式：`@`

```html
<button @click="addCount">+1</button>
```

注意：原生 DOM 对象有 `onclick`、`oninput`、`onkeyup` 等原生事件，替换为 vue 的事件绑定形式后，分别为：`v-on:click`、`v-on:input`、`v-on:keyup`，简写形式分别为 `@click`、`@input`、`@keyup`

##### ④ 双向绑定指令

vue 提供了 v-model 双向数据绑定指令，用来辅助开发者在不操作 DOM 的前提下，快速获取表单的数据。

```html
<p>用户名是：{{ username }}</p>
<input type="text" v-model="username">

<p>选中的省份是：{{ province }}</p>
<select v-model="province">
	<option value="">请选择</option>
	<option value="1">北京</option>
	<option value="2">河北</option>
	<option value="3">黑龙江</option>
</select>
```

注意：只有表单元素使用 v-model 指令才有意义。

##### ⑤ 条件渲染指令

条件渲染指令用来辅助开发者按需控制 DOM 的显示与隐藏。条件渲染指令有如下两个，分别是：

- v-if
  - 原理：每次动态创建或移除元素，实现元素的显示和隐藏。
  - 如果刚进入页面的时候，某些元素默认不需要被展示，而且后期这个元素很可能也不需要被展示出来，此时 v-if 性能更好。
- v-show
  - 原理：动态为元素添加或移除 `display: none` 样式，来实现元素的显示和隐藏。
  - 如果要频繁的切换元素的显示状态，用 v-shiw 性能会更好。

示例用法如下：

```html
<p v-if="flag">请求成功 --- 被 v-if 控制</p>
<p v-show="flag">请求成功 --- 被 v-show 控制</p>

<script>
    ...
    // 数据源
    data: {
		flag: false
	}
    ...
</script>
```

###### v-else：

v-if 可以单独使用，或配合 v-else 指令一起使用：

```html
<div v-if="Math.random() > 0.5">
    随机数大于 0.5
</div>
<div v-else>
    随机数小于或等于 0.5
</div>
```

注意： v-else 指令必须配合 v-if 指令一起使用，否则它将不会被识别！

###### v-else-if：

v-else-if 指令，顾名思义，充当 v-if 的 "else-if 块"，可以连续使用：

```html
<div v-if="type === 'A'">优秀</div>
<div v-else-if="type === 'B'">良好</div>
<div v-else-if="type === 'C'">一般</div>
<div v-else>差</div>
```

注意：v-else-if 指令必须配合 v-if 指令一起使用，否则它将不会被识别！

##### ⑥ 列表渲染指令

vue 提供了 v-for 列表渲染指令，用来辅助开发者基于一个数组来循环渲染一个列表结构。v-for 指令需要使用 item in items 形式的特殊语法，其中：

- items 是待循环的数组
- item 是被循环的每一项

```html
<ul>
    <li v-for="item in list">姓名是：{{ item.name }}</li>
</ul>
<script>
 ...
 data: { 
     list: [ // 列表数据
		{ id: 1, name: 'zs'},
		{ id: 2, name: 'ls'},
     ],
 }
 ...
</script>
```

###### v-for 中的索引：

v-for 指令还支持一个可选的第二个参数，即当前项的索引，语法格式为 `(item, index) in items`，示例代码如下：

```html
<ul>
    <li v-for="(item, index) in list">索引是：{{ index }} ，姓名是：{{ item.name }}</li>
</ul>
<script>
 ...
 data: { 
     list: [ // 列表数据
		{ id: 1, name: 'zs'},
		{ id: 2, name: 'ls'},
     ],
 }
 ...
</script>
```

注意：v-for 指令中的 item 项和 index 索引都是形参，可以根据需要进行重命名。例如 `(user, i) in userlist`

###### key 的注意事项：

1. key 的值只能是字符串或数字类型
2. key 的值必须具有唯一性（即：key 的值不能重复）
3. 建议把数据项 id 属性的值作为 key 的值（因为 id 属性的值具有唯一性）
4. 使用 index 的值当做 key 的值没有任何意义（因为 index 的值不具有唯一性）
5. 建议使用 v-for 指令时一定要指定 key 的值（既提升性能、又防止列表状态紊乱）

```html
<li v-for="(item, index) in list" :key="item.id">
    索引是：{{ index }} ，姓名是：{{ item.name }}
</li>
```

注意：指令是 vue 开发中最基础、最常用、最简单的知识点。

#### 6 使用 JavaScript 表达式

在 vue 提供的模板渲染语法中，除了支持绑定简单的数据值之外，还支持 JavaScript 表达式的运算，例如：

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

#### 7 处理函数的简写

```javascript
// 常规写法
methods: {
	add: function() {
        ...
    }
}
    
// 简写形式
methods: {
    add() {
        ...
    }
}
```

#### 8 通过 this 访问数据源中的数据

```javascript
const vm = new Vue({
	el: '#app',
    data: {
		count: 0
    },
    methods: {
        add() {
            vm.count += 1 // 通过 vm 访问数据源中的数据（不推荐）
            this.count += 1 // 通过 this 访问数据源中的数据（推荐）
        }
    }
})
```

#### 9 $event

vue 提供了内置变量，名字叫做 $event ，它就是原生 DOM 的事件对象 e

```html
<button @click="add($event, 8)">+8</button>
<script>
	...
    methods: {
        add(e, n){ // 接收参数
            this.count += n;
            if(this.count % 2 === 0){
                e.target.style.backgroundColor = 'red'
            }else{
                e.target.style.backgroundColor = ''
            }
        }
    }
    ...
</script>
```

#### 10 事件修饰符

在事件处理函数中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。因此，vue 提供了事件修饰符的概念，来辅助程序员更方便的对事件的触发进行控制。常见的 5 个事件修饰符如下：

| 事件修饰符 | 说明                                                      |
| :--------- | --------------------------------------------------------- |
| .prevent   | 阻止默认行为（例如：阻止 a 链接的跳转、阻止表单的提交等） |
| .stop      | 阻止事件冒泡                                              |
| .capture   | 以捕获模式触发当前的事件处理函数                          |
| .once      | 绑定的事件只触发 1 次                                     |
| .self      | 只有在 event.target 是当前元素自身时触发事件处理函数      |

#### 11 按键修饰符

在监听键盘事件时，我们经常需要判断详细的按键。此时，可以为键盘相关的事件添加按键修饰符，例如：

```html
<!-- 只有在 'key' 是 'Enter' 时调用 'vm.submit()' -->
<input @keyup.enter="submit">

<!-- 只有在 'key' 是 'Esc' 时调用 'vm.clearInput' -->
<input @keyup.esc="clearInput">
```

#### 12 v-model 指令的修饰符

为了方便对用户输入的内容进行处理，vue 为 v-model 指令提供了 3 个修饰符，分别是：

| 修饰符  | 作用                              | 示例                           |
| ------- | --------------------------------- | ------------------------------ |
| .number | 自动将用户的输入值转为数值类型    | <input v-model.number="age" /> |
| .trim   | 自动过滤用户输入的首尾空白字符    | <input v-model.trim="msg" />   |
| .lazy   | 在 "change" 时而非 "input" 时更新 | <input v-model.lazy="msg" />   |

示例用法如下：

```html
<input type="text" v-model.number="n1">
<input type="text" v-model.number="n2">
<span>{{ n1 + n2 }}</span>
```

#### 13 过滤器（vue3 中已弃用）

过滤器（ Filters ）是 vue 为开发者提供的功能，常用于文本的格式化。过滤器可以用在两个地方：插值表达式和 v-bind 属性绑定。

过滤器应该被添加在 JavaScript 表达式的尾部，由“管道符”进行调用，示例代码如下：

```html
<!-- 在双花括号中通过“管道符”调用 capitalize 过滤器，对 message 的值进行格式化 -->
<p>{{ message | capitalize }}</p>

<!-- 在 v-bind 中通过“管道符”调用 formatId 过滤器，对 rawId 的值进行格式化 -->
<div v-bind:id="rawId | formatId"></div>

<script>
	...
    data: {
        message: 'xxx'
    },
	filters: {
        // 注意：过滤器函数形参中的 val ，永远都是 “管道符” 前面的那个值
        capitalize(val) {
            ...
            return ...
        }
    }
</script>
```

##### 全局过滤器

在 filters 节点下定义的过滤器，称为”私有过滤器“，因为它只能在当前 vm 实例所控制的 el 区域内使用。如果希望在多个 vue 实例之间共享过滤器，则可以按照如下的格式定义全局过滤器：

```javascript
// 全局过滤器 - 独立于每个 vm 实例之外
// Vue.filter() 方法接受两个参数：
// 第 1 个参数，是全局过滤器的“名字”
// 第 2 个参数，是全局过滤器的“处理函数”
Vue.filter('capitalize', (val) => {
    return ...
})
```

注意：如果全局过滤器和私有过滤器名字一致，此时按照“就近原则”，调用的是“私有过滤器”。

##### 连续调用多个过滤器

过滤器可以串联地进行调用，例如：

```html
<!-- 把 message 的值，交给 filterA 进行处理 -->
<!-- 把 filterA 处理的结果，再交给 filterB 进行处理 -->
<!-- 最终把 filterB 处理的结果，作为最终的值渲染到页面上 -->
{{ message | filterA | filterB }}
```

##### 过滤器传参

过滤器的本质是 JavaScript 函数，因此可以接收参数，格式如下：

```html
<!-- arg1 和 arg3 是传递给 filterA 的参数 -->
<p>{{ message | filterA(arg1, arg2) }}</p>

<script>
    // 过滤器处理函数的形参列表中：
    // 	第一个参数：永远都是“管道符”前面待处理的值
    // 	从第二个参数开始，才是调用过滤器时传递过来的 arg1 和 arg2 参数
    Vue.filter('filterA', (msg, arg1, arg2) => {
        ...
    })
</script>
```

#### 14 watch 侦听器

watch 侦听器允许开发者监视数据的变化，从而针对数据的变化做特定的操作。语法格式如下：

```javascript
const vm = new Vue({
    el: '#app',
    data: { username: '' },
    watch:{
        // 监听 username 值的变化
        // newVal 是“变化后的新值”，oldVal 是“变化之前的旧值”
        username(newVal, oldVal){
            console.log(newVal, oldVal)
        }
    }
})
```

侦听器常见应用场景：侦听用户名是否被占用。

##### 侦听器的格式

1. 方法格式的侦听器

   - 缺点1：无法在刚进入页面的时候，自动触发！！！
   - 缺点2：如果侦听的是一个对象，如果对象中的属性发生了变化，不会触发侦听器！！！

2. 对象格式的侦听器

   - 好处1：可以通过 `immediate` 选项，让侦听器自动触发！！！

   - 好处2：可以通过 `deep` 选项，让侦听器深度监听对象中每个属性的变化。

   - ```javascript
     watch: {
         // 定义对象格式的侦听器
         username: {
             // 侦听器的处理函数
             handler(newVal, oldVal) {
                 ...
             },
             // immediate 选项的默认值是 false，它的作用是控制侦听器是否自动触发一次
             immediate: true,
             // 开启深度监听，只要对象中任何一个属性变化了，都会触发“对象的侦听器”
             deep: true,
         },
         // 如果要侦听的是对象的子属性的变化，则必须包裹一层单引号
         'info.username'(newVal, oldVal) {
         	...  
         }
     }
     ```



