# Vue 

## props 防止引用传值,而使得对象双向绑定
```js
render: (h) => {
  return h(modelForm, {
    props: {
      data: Object.assign({}, row)
    }
  })
}
```
