---
title: 13个JavaScript数组技巧
date: 2021-09-22 09:19:09
categories:
- JavaScript
tags:
- JavaScript
---

数组是JS最常见的概念之一，它为我们提供了处理存储数据的许多可能性。在刷题过程中常会用的一些技巧，对数组中的元素进行操作。
### 1.数组去重
两种方法：
+ 使用.from()方法
+ 使用拓展运算符...
```javascript
let  fruits = ["banana", "apple", "orange", "watermelon", "apple", "orange", "grape", "apple"]
// 第一种方法
let uniqueFruits = Array.from(new Set(fruits))
// returns [“banana”, “apple”, “orange”, “watermelon”, “grape”]

//第二种方法
let uniqueFruits2 = [...new Set(fruits)]
// returns [“banana”, “apple”, “orange”, “watermelon”, “grape”]
```

### 2.替换数组中特定的值
使用.splice(start , value to remove , value to add),其中三个参数分别指明从哪里开始、要更改多少个值、更改时使用的替换值
```javascript
let  fruits = ["banana", "apple", "orange", "watermelon", "apple", "orange", "grape", "apple"]
fruits.splice(0,2,"potato","tomato")
console.log(fruits) 
//returns  ["potato", "tomato", "orange", "watermelon", "apple", "orange", "grape", "apple"]
```

### 3.不使用.map()映射数组
也许每个人都知道数组的.map()方法，但是可以使用另一种方案来获得相似的效果，并且代码非常简洁。这里我们可用.from()方法。
```javascript
let friends = [
    { name: 'John', age: 22 },
    { name: 'Peter', age: 23 },
    { name: 'Mark', age: 24 },
    { name: 'Maria', age: 22 },
    { name: 'Monica', age: 21 },
    { name: 'Martha', age: 19 },
]

let friendsNames = Array.from(friends, ({name}) => name)

console.log(friendsNames) 
//returns ["John", "Peter", "Mark", "Maria", "Monica", "Martha"]
```
#### 补充知识：使用.map()映射数组
一般写法：
+ 创建Map
+ 将需要查询的数据，按照 "key - value" 的形式存储到map对象中，其中key为关键字，value为查询的信息
+ 查询时根据key查找对应的value
```javascript
let arr = [1,2,3]
let result = arr.map(function(item){
    return item*2
})

console.log(result)
// return [2,4,6]
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210922095015.png)

```javascript
// 箭头函数写法
let result = arr.map(item => {
    return item*2
})

// 去掉花括号的写法（只有一个return的时候，{}可以省略）
let result = arr.map(item => item*2)

// 一个小例子：判断成绩是否及格
let scroe = [12, 77, 88, 99, 33, 100, 59]
let result = scroe.map(item => item >= 60 ? '及格' : '不及格')
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210922095712.png)

### 4.清空数组
清空数组仅需要将数组的长度设为0即可
```javascript
let fruits = ["banana", "apple", "orange", "watermelon", "apple", "orange", "grape", "apple"];

fruits.length = 0;
console.log(fruits); // returns []
```

### 5.数组转对象
数组转换为对象的最快方法是使用扩展运算符...
```javascript
let fruits = ["banana", "apple", "orange", "watermelon"];

let fruitsObj = {...fruits};

console.log(fruitsObj) 
// returns {0: “banana”, 1: “apple”, 2: “orange”, 3: “watermelon”}
```

### 6.用数据填充数组
在刷题时，一般会初始化一个具有相同值的数组。使用.fill()可以快速实现这一需求。
```javascript
let newArray = new Array(10).fill('1') // 数组长度为10，填充内容为‘1’
console.log(newArray) 
// returns [“1”, “1”, “1”, “1”, “1”, “1”, “1”, “1”, “1”, “1”, “1”]
```

### 7.合并数组
两种方法：
+ .concat()方法
+ 扩展运算符...
```javascript
let fruits = ["apple","orange"];
let meat = ["beaf","fish"]
let vegetables = ["potato","cucumber"]

console.log(fruits.concat(meat).concat(vegetables))
// return ["apple", "orange", "beaf", "fish", "potato", "cucumber"]

let food = [...fruits,...meat,...vegetables]
console.log(food)
// return ["apple", "orange", "beaf", "fish", "potato", "cucumber"]
```

### 8.求数组交集
```javascript
var numOne = [0, 2, 4, 6, 8, 8];
var numTwo = [1, 2, 3, 4, 5, 6];
var duplicatedValues = [...new Set(numOne)].filter(item=> numTwo.includes(item))
console.log(duplicatedValues); // returns [2, 4, 6]
```

### 9.从数组中删除虚值
在Javascript中，虚值有false, 0, „”, null, NaN, undefined。现在，我们可以找到如何从数组中删除此类值。为此，我们将使用.filter（）方法。
```javascript
var mixedArr = [0, “blue”, “”, NaN, 9, true, undefined, “white”, false];
var trueArr = mixedArr.filter(Boolean);
console.log(trueArr); // returns [“blue”, 9, true, “white”]
```

### 10.从数组中获取随机值
可以根据数组长度获取随机索引号
```javascript
var colors = ["blue", "white", "green", "navy", "pink", "purple"];

var randomColor = colors[(Math.floor(Math.random() * (colors.length)))]
```

### 11. 反转数组
使用.reverse()方法
```javascript
var colors = ["blue", "white", "green", "navy", "pink", "purple"];
var reversedColors = colors.reverse();
console.log(reversedColors); 
// returns ['purple', 'pink', 'navy', 'green', 'white', 'blue']
```

### 12.lastIndexOf（）方法
lastIndexOf（）方法:查找给定元素的最后一次出现的索引。
```javascript
var nums = [1, 5, 2, 6, 3, 5, 2, 3, 6, 5, 2, 7];
var lastIndex = nums.lastIndexOf(5);
console.log(lastIndex); // returns 9
```

### 13.求数组中的所有值的和
```javascript
var nums = [1, 5, 2, 6];
var sum = nums.reduce((x, y) => x + y);
console.log(sum); // returns 14
```