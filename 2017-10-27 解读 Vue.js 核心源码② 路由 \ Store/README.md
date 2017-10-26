# 类型判断

``Array.isArray``方法可以判断目标是否为数组：

```js
var result = Array.isArray( [1, 2, 3] )
console.log(result)  // true
```


# 索引

ES5标准中，``Array``对象终于加入了``indexOf``与``lastIndexOf``方法，它们能找出元素所在下标值。国际惯例，indexOf从前往后找，lastIndexOf从后往前找：

```js
var array = ['a','b','c','d','a','b','c','d']
array.indexOf('c')      // -> 2
array.lastIndexOf('c')  // -> 6
array.indexOf('c', 5)      // -> 6，从array[5]的'a'开始往后找
array.lastIndexOf('c', 5)  // -> 2，从array[5]的'a'开始往前找
```


# 迭代

``Array``还添加了一系列迭代方法，以适应越来越多的数据处理需求，它们包括``forEach``、``map``、``filter``、``every``、``some``、``reduce``、``reduceRight``。


## forEach

``forEach``遍历数组中每个元素，无返回值，是``for( var i = 0; i < array.length; i++ )``的简洁版，可读性更强：

```js
var array = ['a','b','c','d','e']
array.forEach(function(value, index, array){
  console.log('value: ' + value + ' | index: ' + index)
})

// -> value: a | index: 0
// -> value: b | index: 1
// -> value: c | index: 2
// -> value: d | index: 3
// -> value: e | index: 4
```

被迭代的数组会作为回调的第三个参数传入。

还可以传入``thisObj``来绑定回调的``this``：

```js
var array = ['a', 'b', 'c', 'd', 'e']
var thisObj = { name: 'kid' }
array.forEach(function(value){
  console.log(this)
}, thisObj)

// -> Object { name: 'kid' }
// -> Object { name: 'kid' }
// -> Object { name: 'kid' }
// -> Object { name: 'kid' }
// -> Object { name: 'kid' }
```

所有这些新加入的迭代方法，都可以拿到``index``、``array``参数，也可以绑定``this``，所以之后不再赘述。


## map

``map``将数组映射为一个新数组：

```js
  var array = ['a','b','c','d','e']
  var new_array = array.map(function(value){
    return value + '!'
  })

  console.log(array)
  // -> ['a','b','c','d','e']

  console.log(new_array)
  // -> ['a!','b!','c!','d!','e!']
```

最终会返回一个新数组，原数组保持不变。


## filter

``filter``设置关卡、过滤元素：

```js
  var array = ['a','b','c','d','e']
  var new_array = array.filter(function(value){
    if( value !== 'b' && value !== 'd' ){
      // 返回true表示通关
      return true
    }
  })

  console.log(array)
  // -> ['a','b','c','d','e']

  console.log(new_array)
  // -> ['a','c','e']
```

最终会返回一个新数组，由原数组中通关的元素构成。


## every

``every``类似“逻辑与”运算，用来判断所有元素是否都符合某个规则，只有每次迭代都返回``true``，``every``整体才返回``true``，比如检查数组中是否都为奇数：

```js
  function isOdd(value){
    return value % 2 === 1
  }

  var result_1 = [1,3,5].every( isOdd )  // -> true
  var result_2 = [1,3,6].every( isOdd )  // -> false
```

注意，``every``从性能上考虑而支持逻辑短路，当发现某次迭代返回了``false``，就不会继续之后的迭代了。


## some

``some``类似“逻辑或”运算，只要至少有一次迭代返回``true``，那么``some``整体就返回``true``，比如我们想检查水果数组中是否包含苹果：

```js
  function isApple(value){
    return value === 'Apple'
  }

  var result_1 = ['Apple','Orange','Banana'].some( isApple )  // -> true
  var result_2 = ['Lemon','Orange','Banana'].some( isApple )  // -> false
```

``some``也支持短路，比如在上面的例子中，第一个数组的首个元素就是``Apple``，那么仅仅将执行一次迭代。


## reduce / reduceRight

``reduce``对数组求值，从第二个元素（下标1）开始迭代，为了解释明白，我们先将回调的参数打印出来看看：

```js
  var array = ['a','b','c','d','e']
  array.reduce(function( prev_return, value ){
    console.log( prev_return, value )
    return prev_return + value
  })
  // -> 'a b'
  // -> 'ab c'
  // -> 'abc d'
  // -> 'abcd e'
```

其中，``prev_return``是上一次迭代的返回值，第一次迭代的``prev_return``为第一个元素``"a"``。

如果你了解``MapReduce``算法的过程，那么就能理解``reduce``在收敛求值方面的作用，一个没什么实际价值的小例子：

```js
  var array = ['a','b','c','d','e']
  var result = array.reduce(function( prev_return, value ){
    return prev_return + value
  })
  console.log( result )  // -> 'abcde'
```

``reduceRight``则正好相反，从后向前迭代：

```js
var array = ['a','b','c','d','e']
var result = array.reduceRight(function( prev_return, value ){
  return prev_return + value
})
console.log(result)  // -> 'edcba'
```

我们还可以显式指定第一次迭代的``prev_return``值：

```js
var array = ['a','b','c','d','e']
var result = array.reduce(function( prev_return, value ){
  return prev_return + value
}, 'word: ')
console.log(result)  // -> 'word: abcde'
```



> 原创，自由转载，请署名，[本人博客 kid-wumeng.me](http://kid-wumeng.me)