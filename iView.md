# iView 按需引用

全部引用iView, npm run build 以后, 多了900k, 

按照官网说的按需引用自己想要的组件,发现有些组件有坑.

而且一个个添加好麻烦呀.

## 解决思路:

直接去node_modules\iview\src\index.js 下吧不需要的组件import 注释掉

不过这样更新以后,就覆盖掉了

所以需要以下步骤:

1. 新建目录 src/components/iview

2. 把 node_modules\iview\src\index.js 拷贝到新建目录iview下

3. 编辑 index.js 
```
  ./components/ 替换为 iview/src/components/
```
4. 编辑src/main.js
```
import iView from 'iview' 改成 import iView from './components/iview'
```
