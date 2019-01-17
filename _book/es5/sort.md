本篇讲述了sort()方法的用法，参数以及排序原理
`sort()`方法用于对数组元素进行排序，并返回数组
语法：`arrayObject(sortBy)`,sortBy 可选，是一个函数参数
#### 1. 默认情况下
如果没有传递参数，那么将按照字母顺序对数组中的元素进行排序，即按照字符编码的顺序进行排序
### 2. 在有函数参数的情况下
  * 升序
  ```js
  function sortNumber(a,b)
  {
  return a - b
  }

  var arr = new Array(6)
  arr[0] = "10"
  arr[1] = "5"
  arr[2] = "40"
  arr[3] = "25"
  arr[4] = "1000"
  arr[5] = "1"

  document.write(arr + "<br />")
  document.write(arr.sort(sortNumber))

  ```
  * 降序
  ```js
    function sortNumber(a,b) {
    return b - a
    }

  ```
