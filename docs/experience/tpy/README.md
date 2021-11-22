## promise.all的实现
```js
Promise.all = function (arr) {
  return new Promise((resolve, reject) => {
    const ans = [];
    let index = 0;
    for (let i = 0; i < arr.length; i++) {
      arr[i].then(res => {
        ans[i] = res;
        index++;
        if (index === arr.length) {
          resolve(ans)
        }
      }).catch(err => reject(err))
    }
  })
}
```
扩展 Promise.race
```js
Promise.race = function (arr) {
  return new Promise((resolve, reject) => {
    arr.forEach(p => {
      Promise.resolve(p).then(val => resolve(val), err => reject(err))
    })
  })
}

```
## 字符串千分符

## 字符串出现最多的三个

## 浏览器的缓存机制

## transform animation区别

## 伪元素