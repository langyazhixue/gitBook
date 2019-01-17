# 递归算法，js实现
递归，就我而理解的定义式这样的：是将问题转化为规模缩小的同类问题的子问题，每一个子问题都用同一个同样的算法去解决。一般来说，一个递归算法就是函数调用自身去解决它的问题。<br/>
递归算法的特点：
1. 在函数过程中调用自身
2. 在递归过程中，必须有一个明确的条件判断递归的结束，即递归出口
3. 递归算法简洁但效率低，通常不作为推荐算法。

****
## 阶乘
问题描述： n! = n*(n-1)*...2*1
代码实现：
```js
  var factorial = function (n) {
  if (n <= 1) {
    return 1
  } else {
    return n * factorial(n * 1)
  }
}
console.log(factorial(5)) // 120

```
拿到问题的时候，我们按照定义的说明，可以先将`规模缩小到同类的子问题`。比如，n! 是等于 n* (n-1)!，然后(n-1)! = (n-1)*(n-2)!。这样依次往下推，直到if的出口。函数实现起来是不是简洁明了呢。当然因为问题规模简单，其实用循环也是可以实现的，大家可以尝试一下。
****

## 斐波那契数列
问题描述：1,1,2,3,5,8,13,.... 求第n个数是多少。
代码实现：

```js
var fibonacci = function (n) {
  if (n < 0) {
    return 1
  }
  if (n <= 2) {
    return 1
  }
  return fibonacci(n - 1) + fibonacci(n - 2)
}
console.log(fibonacci(8)) // 21
```
通过分析可以得到第n个数，是前两个数的和，通过这个我们就可以通过递归，不断获得所需要的前两个数，直到n<= 2这个条件返回1

***

## 项目中遇到的一个问题
问题描述：一个对象中有users,groups字段，groups是数组，数组中的每一个对象中有users和gruops，最后要把所有的users 拿出来
数据结构如下：
```js
var data = {
  users: [{
    name: 'test0',
    id: 0
  }, {
    name: 'test1',
    id: 1
  }],
  groups: [
    {
      users: [{
        name: 'test2',
        id: 2
      }, {
        name: 'test3',
        id: 3
      }]
    }, {
      users: [{
        name: 'test4',
        id: 4
      }, {
        name: 'test5',
        id: 5
      }],
      groups: [
        {
          users: [{
            name: 'test6',
            id: 6
          }, {
            name: 'test7',
            id: 7
          }]
        }, {
          users: [{
            name: 'test8',
            id: 8
          }, {
            name: 'test9',
            id: 9
          }]
        }
      ]
    }
  ]
}

```
代码如下：
```js
  let arr = []
  function factorial (data) {
    if (data.users.length > 0) {
      arr = arr.concat(...data.users)
    }
    if (data.groups.length > 0) {
      data.groups.map(item => {
        if (item.groups && item.groups.length > 0) {
          factorial(item)
        } else {
          arr = arr.concat(...item.users)
        }
      })
    }
  }
  factorial(data)
```
通过分析我们可以看到,groups数组中的对象有可能还有groups,那么针对判断有groups的对象进行一次递归就可以了

*** 

## 快排
- 问题描述：使用二分法，对一个数组进行由小到大的排序。
```js
function quikSort (arr) {
  if (arr.length <= 1) {
    return arr
  }
  var baseIndex = Math.round(arr.length / 2)
  // 删除一个
  var baseValue = arr.splice(baseIndex, 1)[0]
  // splice 以数组形式返回删除的成员
  var left = []
  var right = []
  for (var i = 0; i < arr.length; i++) {
    if (arr[i] < baseValue) {
      left.push(arr[i])
      continue
    }
    right.push(arr[i])
  }
  return quikSort(left).concat(baseValue, quikSort(right))
}

```
