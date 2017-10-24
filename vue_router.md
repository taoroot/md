# vue router

## router 拦截

未登录状态,强制调转到登录界面

```javascript
// router/index.js
import Vue from 'vue'
import Router from 'vue-router'
import routes from './routes'

Vue.use(Router)

const router = new Router({
  routes
})

router.beforeEach((to, from, next) => {
  // 路由认证
  if (to.meta.requiresAuth) {
    // 判断有误token,没有的话要求跳转到登陆界面
    var token = '将这里换成获取 token 的方法'
    if (token) {
      // 有token, 继续执行
      next()
    } else {
      // 无token, 将跳转到登录界面,并同时记录下当前的路径地址,登录以后可以直接跳回
      next({
        path: '/login',
        query: {
          redirect: to.fullPath
        }
      })
    }
  } else {
    // 无需认证
    next()
  }
})
export default router
```

```js
// router/router.js
import Home from '@/components/layout/Home'
import Index from '@/components/views/Index'
// 测试页面
const NotFound = r => require.ensure([], () => r(require('@/components/pages/NotFound')))
const Register = r => require.ensure([], () => r(require('@/components/pages/Register')))
const E404 = r => require.ensure([], () => r(require('@/components/pages/E404')))
const Icon = r => require.ensure([], () => r(require('@/components/pages/Icon')))
const Login = r => require.ensure([], () => r(require('@/components/pages/Login')))
const Filter = r => require.ensure([], () => r(require('@/components/pages/Filter')))

const routes = [
  {
    path: '/',
    name: 'home',
    component: Home,
    redirect: '/index',
    children: [
      {
        path: '/index',
        name: 'index',
        component: Index,
        meta: {
          requiresAuth: true, // 是否需要登录
        }
      },
  },
  {
    path: '/login',
    name: 'login',
    component: Login
  },
  {
    path: '/register',
    name: 'register',
    component: Register
  },
  {
    path: '/error404',
    name: 'error404',
    component: E404
  },
  {
    path: '*',
    redirect: '/notFound'
  }
]

export default routes

```
### 总结
vue router 只是做到了前端路由拦截, 如果用户强行设置token,这里的功能就失效了,所以还需要
再加一个http请求拦截,这个在 vue_axios.md 中说明.
