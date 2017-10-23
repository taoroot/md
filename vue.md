# Vue.js
## 介绍
一套构建用户界面的渐进式框架。
采用自底向上增量的设计。

## 声明式渲染
Vue 核心是一个允许采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统
```html
<div id="app-2">
  <!-- 将这个元素节点的 title 属性和 Vue 实例的 message 属性保持一致 -->
  <span v-bind:title="message">
    鼠标悬停几秒，看见动态绑定的内容
  </span>
</div>
```
```js
var app = new Vue({
  el: '#app',
  data: {
    message: '页面加载于 ' + new Date().toLocalString
  }
})
```

## 条件与循环
控制切换一个元素的显示
```html
<div id="app">
  <!-- 绑定 DOM 结构到数据 -->
  <p v-if="seen">现在你看到我了</p>
</div>
```
```js
var app = new Vue({
  ele: '#app',
  data: {
    seen: true
  }
})
```
