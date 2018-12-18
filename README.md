### 创建实例
+ `el` 指定当前 vue 实例“管控”的区域
### 常用指令
+ `v-cloak`可以解决插值表达式 `{{}}` **渲染闪烁**的问题
  - [官方文档](https://cn.vuejs.org/v2/api/#v-cloak '点击查看vue.js官方文档')
  - `v-cloak`必须结合 css 属性选择器 `[v-cloak]{ display: none; }` 一起使用；
    ```css
    /* css */
    [v-cloak]{
      display: none;
    }
    ```
    ```html
    <!-- html -->
    <div id="test">
      <p v-cloak>{{ msg }}</p>
    </div>
    ```
    ```javascript
    // javascript
    var vm = new Vue({
      el: '#test',
      data() {
        return {
          msg: '我是data中定义的msg'
        }
      }
    })
    ```
+ `v-text` 默认是没有闪烁问题的；
+ `v-text` 会覆盖元素中原本的内容, 但是插值表达式只会替换自己的占位符, 不会把整个元素的内容清空；
+ `v-bind` 是用于绑定属性的指令, 可以写合法的 js 表达式；
+ `v-bind:title="myTitle + '自定义字符串'";` myTitle 相当于一个变量；
+ `v-on:click="showTitle('自定义标题的内容')"`。

### 事件修饰符
+ `.stop` 阻止事件冒泡（向上冒泡）；
+ `.self` 只有当前事件对象是自身时, 才会触发事件（**不会阻止事件向上冒泡, 只是自己不触发**）；
+ `.prevent` 阻止原生组件的默认行为, 如 a 链接的跳转、表单的提交等等；
+ `.capture` 捕获处理事件, 事件**从外到内**触发；
+ `.once` 事件只触发一次。


### 数据绑定
+ 数据改变, 页面更新, 这是单向。`v-bind`；
+ 页面改变, 数据也改变, 这是双向。`v-model` (运用在表单元素中)。

### class 绑定
+ 数组：绑定多个类名。`:class="['red', 'blue', 'green']"`；
+ 数组使用场景：绑定的样式是多个 classObj 时, 使用数组可以合并；
+ `:class="red"` 与 `:class="'red'"` 不同, 前者 red 是一个变量；
+ 对象：动态绑定。`:class="{'red': isTrue}"` 可以和数组一起用；
+ `:class="{red: isRed, blue: isBlue, green: isGreen}"` 类名也可以不写引号 ；
+ `{red: isRed, blue: isBlue, green: isGreen}` 也可以定义成 data 中的变量。

### v-for 的 key
+ `key` 字符串/数值类型。不能使用复杂类型（例如对象、数组等）；
+ `key` 用来标记当前循环项的唯一身份。将当前数据与页面对应起来, 不指定会怎么样呢？？

### vue-devtools 安装
+ 通过浏览器插件的方式安装（ google 需要翻墙）；
+ 通过本地文件安装, 安装后需要勾选上“**允许访问文件网址**”。

### indexOf 和 includes
+ `list.forEach(item => {})` forEach() 遍历
+ `'班主任'.indexOf('')` 结果是0, 不是-1。
+ ES6 提供了一个新的方法：**`String.prototype.includes(keywords)`**

### 过滤器
过滤器一定要有返回值
```html
<div id="test">
  <p>过滤前：{{msg}}</p>
  <p>过滤后：{{msg | filterSB}}</p>
</div>
```
```js
var vm = new Vue({
  el: '#test',
  data() {
    return {
      msg: '看看能不能过滤煞笔'
    }
  },
  // 私有过滤器
  filters: {
    filterSB(value) {
      // console.log(value);
      // 过滤器一定要有返回值, 否则页面无内容
      return value.replace(/煞笔/g, '**');
    }
  }
});
```
##### 1. 全局过滤器
+ 过滤器：在输出之前, 做最后一层的处理。
+ `Vue.filter(过滤器的名称, function() {})` 定义全局的过滤器；
  - 可以定义全局的过滤器, 例如格式化时间
+ `{{ data | dataFilter}}` |: 管道符；
+ `msg.replace('单纯', '邪恶')` replace() 的第一个参数除了是字符串外, 还可以是一个正则表达式；
+ `msg.replace(/单纯/g, '邪恶')` 正则可以匹配多个；
+ 过滤器可以多个联合使用 `{{msg | filter1 | filter2}}` 原始的 msg 先经过 filter1 的处理, 处理后的结果在交由 filter2 处理；
+ 过滤器也可以传参, 它就是相当于一个函数的调用 `{{date | formatDate(yyy-mm-dd)}}`；
+ `&&`是个好东西, 能规避很多数据为空时的错误；
+ Vue 全局, 就是所有的 Vue 实例都能够共享的。 #app1 和 #app2...

##### 2. 私有过滤器
+ filter 作为 `new Vue({ filters: {}})` 的参数对象一个字段
  - 全局定义指令时, 一次只定义一个, 所以 filter 是单数；但是在私有中定义时, filters 是一个复数
+ 过滤器调用的时候, 采用就近原则。优先调用私有过滤器, 再是全局
+ ES6的新方法：**`String.prototype.padStart(length, str)`** // length: 填充后的字符串长度, 填充的字符串（可以给时间补0）
  - [String.prototype.padStart()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padStart 'MDN官方文档')


### 键盘修饰符
+ `keyup.keyNumber` (键盘按键对应的键盘值)
+ `Vue.config.keyCodes.f2(名称) =  113(键盘值)` // 自定义全局的键盘修饰符, 使得代码可读性更高

### 自定义指令
+ 在vue中, 所有的指令, 都以v-开头。
+ 在定义指令的时候, name 不需要加 "v-" 前缀, 在调用的时候, 需要添加前缀哦
+ `Vue.directive(name, { vue提供的函数 })`
+ bind: 每当指令绑定到元素上的时候, 会立即执行这个函数, **只执行一次, 此时 el 还没有插入到 DOM 中, 在内存中**
+ `inserted` 表示元素插入到 **DOM** 中的时候, 会执行该函数, **只执行一次**
+ `updated` 当 VNode 更新的时候, 会执行, 可能会执行多次哦~
+ `el` 被绑定了该指令的**原生 js 对象**
+ 只要给元素绑定了指令, 不管这个元素有没有被插入到页面中, 这个元素的**内联样式**是一定存在的
+ 和 el 的 js 相关的操作, 要在 inserted 中；和 el 样式相关的操作, 可以在bind中进行。
+ binding对象 `v-add:c.a.b = "1+1"`
  - `binding.name` 指令的名称（add）；
  - `binding.value` 指令的值（2）；
  - `binding.expression` 绑定值的字符串形式（1+1）；
  - `binding.oldValue` 绑定值的前一个值, 在 `update` 和 `componentUpdated` 时可用；
  - `binding.arg` 指令的参数。**指令的参数要紧跟指令, 不能在修饰符后。如`v-add.a.b:c`。因为参数是直接修饰指令的~**；
  - `binding.modifiers` 指令的修饰符对象 `{a:true, b: true}`；
+ el 和 binding 是参数的形参, 可以自定义, 但是不建议瞎改；
+ `new Vue({ directives: {} })` 定义私有指令。与 filters 一样, 也是复数, 需要添加s；
+ 如果处理函数应用在 bind 和 update 时, 可以简写。


### 生命周期
+ 生命周期函数 = 生命周期钩子 = 生命周期事件
![vue实例的生命周期](https://cn.vuejs.org/images/lifecycle.png)
+ 组件创建期间的生命周期函数：
  - `new Vue({})` 表示开始创建一个 Vue 的实例对象；
  - `Init Events & Lifetcycle` 初始化了一个 Vue 空实例对象, 这个对象身上, **只有默认的一些生命周期函数和默认的事件**, 其他的东西都未创建；
  - `beforeCreate` 在 beforeCreate() 执行时, data 和 methods 中的数据都还没有初始化；
  - `Init Injections & reactivity` 初始化数据；
  - `created` 此时 data 和 methods 都已经完成初始化（最早）；
  - `Has 'el' option? Has 'template' option?` 开始编译模板, 然后**在内存中生成对应的DOM**, 并没有挂载在页面中；
  - `beforeMount` 模板已经在内存中完成渲染, 但是并未渲染到页面中。**此时可以获取到元素, 但是元素在内存中。**,  与自定义指令的 bind 和 inserted 类似。获取到的元素内容是 {{msg}} , **因为渲染好的最新的模板（带数据的）还在内存中**；
  - `mounted` 内存中的模板, 已经真实地挂载到了页面中；// 如果要操作 dom , 最早在此
+ 组件运行阶段的生命周期函数：
  - 最少执行0次, 只有在 data 改变时, 才会触发以下两个函数。
  - `beforeUpdate` data 中的数据更新了（只有 data 改变时才触发嘛）, 但是视图还未更新；
  - `Virtual DOM re-render and patch` 根据 data 中最新的数据, 重新渲染出一份最新的内存 DOM 树；**当最新的内存DOM树更新之后, 会把最新的内存DOM树重新渲染到真实的页面中去。**就完成了数据（data）=>  视图（view）的更新
  - `update` 页面中的数据已经和 data 中的数据同步；
+ 组件销毁期间的生命周期函数：
  - `beforeDestroy` Vue 实例已经从运行阶段, 进入到了销毁阶段。**组件身上所有的 data、methods、filters 等都是可用的, 还未真正销毁**；
  - `destroyed` 组件实例已经被销毁, 所有的属性方法已经不可用。

### 数据请求
+ post 请求：`application/x-www-form-urlencoded`
  -手动发起的请求默认没有表单格式, 所以, 有的服务器处理不了
+ jsonp: 只支持 GET 请求, 实际上是动态创建 script 标签（不会有跨域问题）
  - 服务器可能不知道你的 callback 的名称, 可以通过参数传过去。
  - `<script src="https://xxx.com/api?callback=callbackName"></script>  =>  <script>callbackName()</script>`
    - callbackName 就是预先定义好的回调方法, 返回数据后会调用这个函数。