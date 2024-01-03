---
title: "Solidity学习笔记\U0001F4D2"
date: 2023-12-29 15:01:06
tags:
- Solidity笔记
---
## 1.Solidity是什么？

1. Solidity是一种面向对象（合约）的，为实现智能合约而创建的高级编程语言；
2. Solidity是一种针对以太坊虚拟机（EVM）设计的语言，它受C++、Python和JavaScript的影响；
3. Solidity是一种静态类型语言，支持复杂的用户定义编程，支持库和集继承。

**合约样例**

```solidity
// 版本许可：版本许可位于源文件的第一行，用于定义合约的版权许可标识。虽然不是强制的，但是建议每个源文件中都以这样的代码开始，来说明合约的版本许可。
// SPDX-License-Identifier:MIT
pragma solidity ^0.8.1; //版本标识

// 第一个合约
contract HelloWorld{
	// 状态变量
	string public str = "Hello World";
	
	// set函数
	function set(string memory s) public{
		str = s;
	}
	
	//get函数
	function get() public view returns(string memory){
		return str;
	}
}
```

*版本标识的说明*

![image-20240102110415767](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240102110415767.png)

## 2.数据类型

### 基本概念

Solidity中关于数据类型的定义如下：

+ Solidity是一种静态类型的语言，这意味着每个变量都需要在编译时指定变量的类型；
+ Solidity中新声明的变量总是有一个默认值，具体默认值跟类型有关，例如boolean类型的默认值为false.

### 数据类型

#### 值类型

**布尔类型**

`bool`：可接受`true`和`false`两个值，默认值是`false`

**整型**

`int`和`uint`：分别表示有符号和无符号的整数，默认为0。支持关键字`int8`到`int256`，以及`uint8`到`uint256`，从8位到256位，以8位为步长递增，`int`和`uint`分别是`int256`和`uint256`的别名。

**地址类型**

`address`：包含一个20字节的值（代表一个以太坊地址的大小）。一个地址可以用来获取余额，也可以通过转账的方式来转移余额。

**字节类型**

`bytes1` `bytes2` ……`bytes32`：字节用于存储固定大小的字符集，长度范围是1-32。字节的一个优点是它使用更少的Gas，所以当我们知道数据的长度的时，最好使用它。

**字符串类型**

`string`：字符串用于存储等于或大于一个字节的字符集，字符串的长度是动态的。

**枚举类型**

`enum`：创建用户定义的数据类型，用于为一个整型常量分配一个名称，这使得合约具有可读性、可维护性和更不容易出错。枚举的选项可以用从0开始的无符号整数值表示。

语法：`enum <enum_name> {element1, element2, ...}`

#### 例子🌰

```solidity
// SPDX-License-Identifier:MIT
pragma solidity ^0.8.13;//版本标识

// 值类型
contract DataTypes{
    // 布尔类型
    bool public boo = true;

    // 整型
    uint8 public u8 = 123;
    uint256 public u256 = 456;
    uint public u = 789;

    int8 public i8 = -1;
    int256 public i256 = -456;
    int public i = -789;

    // 整型的最大值和最小值
    int public minInt = type(int).min;
    int public maxInt = type(int).max;

    // 地址类型
    address public addr = 0xB5893179159A988f19B900A0f264cf2e5fBff829;
    uint public balance = addr.balance;

    // 字节类型
    bytes1 public b1 = 0x1a;
    bytes2 public b2 = 0x1a2b;

    // 字符串类型
    string public str = "this is a string";

    // 默认值
    bool public defaultBoo;
    uint public defaultUint;
    int public defaultInt;  
    address public defaultAddr;
}
```

```solidity
// 枚举类型
// SPDX-License-Identifier:MIT
pragma solidity >=0.5.10;//版本标识

contract Enum{
    // 定义一个枚举类型
    enum Action {up,down,left,right}

    // 定义变量，默认值为第一个元素，即up的值
    Action public action;

    // 设置默认值
    function setDefault() public {
        action = Action.up;
    }

    //设置传递一个uint的值
    function set(Action _action) public {
        action = _action;
    }

    // 取值，返回一个uint值
    function get() public view returns(Action) {
        return action;
    }

    // 取最小值
    function getMinValue() public pure returns(Action) {
        return type(Action).min;
    }

    // 取最大值
    function getMaxValue() public pure returns(Action) {
        return type(Action).max;
    }
}
```

#### 引用类型

引用类型变量存储数据的位置。在引用类型的定义中，两个不同的变量可以引用同一个位置，其中一个变量的任何更改都会影响另一个变量。引用类型包括数组、结构和映射。

##### 数组

**基本概念**

solidity中关于数组的定义如下：

+ 数组是存储相同数据类型的固定元素集合的数据结构；
+ 数组可以在声明时指定长度，也可以动态调整大小（长度）；
+ 数组具有连续的内存位置，通过索引访问数组中的元素，索引从0开始；
+ 数组元素可以是任何有效的Solidity数据类型，包括映射和结构体。

**创建数组**

语法：`<data type>[size] <array name> = <initialization>`

**数组类型**

1⃣️定长数组：数组的大小在声明时预定义，元素的总数不应该超过数组的大小。如果数组在声明时没有进行初始化，则数组中的元素为默认值（如对于存储整型的数组，其元素的默认值为0）

定长数组的声明与初始化有两种方法：

```solidity
uint[] a = [1,2,3]
uint[3] b;
```

数组a定义了3个元素，并初始化为[1,2,3]

数组b定义了3个元素，所有元素的初始化为0



2⃣️动态数组：声明数组时，数组的大小没有预定义。数组的大小会随着元素的添加、删除而改变，在运行时数组的大小将被确定。

动态数组的声明如下：`uint[] a;`

动态数组由于没有指定数组长度，所以没有初始化值。



3⃣️内存数组：可使用关键字`new`在内存中（memory）中基于运行时动态创建固定长度的数组。与存储（storage）数组相反的是，不能通过修改成员变量`.push`改变内存数组的大小，即内存数组创建后的长度是固定的。

创建内存数组的语法如下：`unit[] memory a = new uint[](5);`

动态数组中的元素总是以默认值初始化

**数组成员**

**length**

数组的`length`变量用于检查数组中存在的元素的数量。定长数组的大小在声明时是固定的，而如果动态数组是在定义时定义的，则需要操作长度。

**push(x)**

用于在动态数组中添加新元素。新元素总是添加在数组的最后一个位置。如果带`x`参数则向动态数组添加定值元素，并且没有返回。如果不带参数`x`，则向数组添加初始化元素，并返回元素的引用。

**pop()**

用于从动态数组末尾移除元素，并在移除的元素上隐含调用`delete`









