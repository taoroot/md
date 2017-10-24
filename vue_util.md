# vue uitl 
```js

/**
 * 常用方法工具
 * util/index.js
 */
/**
 * 获取地址栏参数值
 * @param name
 * @returns {null}
 * @constructor
 */
export function GetQueryString (name) {
  let reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)')
  let r = window.location.search.substr(1).match(reg)
  if (r != null) return unescape(r[2])
  return null
}

/**
 * 获取cookie
 * @param name
 * @returns {tips}
 * @constructor
 */
export function GetCookie (name) {
  let getCookie = document.cookie.replace(/[ ]/g, '')
  let arrCookie = getCookie.split(';')
  let tips
  for (let i = 0; i < arrCookie.length; i++) {
    let arr = arrCookie[i].split('=')
    if (name === arr[0]) {
      tips = arr[1]
      break
    }
  }
  return unescape(tips)
}

/**
 * 设置cookie
 * @param name, val, time
 * @returns {null}
 * @constructor
 */
export function SetCookie (name, val, time) {
  let date = new Date()
  // 将date设置为n天以后的时间
  let expiresDays = time
  // 格式化为cookie识别的时间
  date.setTime(date.getTime() + expiresDays * 24 * 3600 * 1000)
  // 设置cookie
  document.cookie = name + '=' + escape(val) + ';expires=' + date.toGMTString()
}

/**
 * 删除cookie
 * @param name
 * @returns {null}
 * @constructor
 */
export function DelCookie (name) {
  let exp = new Date()
  exp.setTime(exp.getTime() - 1)
  let cval = GetCookie(name)
  if (cval != null) {
    document.cookie = name + '=' + cval + ';expires=' + exp.toGMTString()
  }
}
```
