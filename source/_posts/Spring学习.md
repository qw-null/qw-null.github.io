---
title: Spring学习
date: 2023-08-16 10:47:56
tags:
  - Java
---

[黑马程序员新版 Spring 零基础入门到精通，一套搞定 spring 全套视频教程（含实战源码）](https://www.bilibili.com/video/BV1rt4y1u7q5/?share_source=copy_web&vd_source=f3ff8a2761a3e07339584c4852cdd504)

# 课程大纲

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161051390.png)

# 第一部分：IoC 基础容器

## 1.1 传统 Java Web 开发的困惑

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161059117.png)
**问题 1:** 层与层之间紧密耦合在一起，接口与具体实现紧密耦合在一起

解决思路：程序代码中不要手动 new 对象，第三方根据要求为程序提供需要的 Bean 对象
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161105786.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161110130.png)
**问题 2:** 通用的事务功能耦合在业务代码中，通用日志功能也耦合在业务代码中

解决思路：程序代码中不要手动 `new` 对象，第三方根据要求为程序提供需要的 `Bean` 对象的代理对象
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161112204.png)

## 1.2 三种思想

**IoC 思想：** Inversion of Control，控制反转，强调的是原来在程序中创建 Bean 的权利反转给第三方。

**DI 思想：** Dependency Inject，依赖注入，强调的 Bean 之间的关系，这种关系第三方负责去设置。

**AOP 思想：** Aspect Oriented Programming，面向切面编程，功能的横向抽取，主要实现方式就是`proxy`。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161127582.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161128255.png)

## 1.3 Spring 框架

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161439776.png)

### 1.3.1 BeanFactory 快速入门

Spring 的 BeanFactory 开发步骤：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161526507.png)

（1）导入`Spring`的`jar`包或`Maven`坐标；
（2）定义`UserService`接口及其`UserServiceImpl`实现类；
（3）创建`beans.xml`配置文件，将`UserServiceImpl`的信息配置到该`xml`中；
（4）编写测试代码，创建`BeanFactory`，加载配置文件，获取`UserService`实例对象。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308161652965.png)

### 1.3.2 DI 依赖注入的过程：

在上述 1.4 节中使用`BeanFactory`完成了`IoC`思想的实现，下面是实现`DI`依赖注入：
（1）定义`UserDao`接口及其`UserDaoImpl`实现类；
（2）修改`UserServiceImpl`代码，添加一个`setUserDao`（`UserDao userDao`）用于接收注入的对象；
（3）修改`beans.xml`配置文件，在`UserDaoImpl`的`<bean>`中嵌入`<property>`配置注入；
（4）修改测试代码，获得`UserService`时，`setUserService`方法执行了注入操作。

### 1.3.3 ApplicationContext 快速入门

`ApplicationContext`称为`Spring`容器，内部封装了`BeanFactory`，比`BeanFactory`功能更丰富强大，使用`ApplicationContext`进行开发时，`xml`配置文件的名称习惯写成`applicationContext.xml`

```java
// 创建ApplicationContext，架子啊配置文件，实例化容器
ApplicationContext applicationContext = new ClassPathxmlApplicationContext("applicationContext.xml");
// 根据beanName获得容器中Bean实例
UserService userService = (UserService) applicationContext.getBean("userService");
System.out.println(userService);
```

### 1.3.4 BeanFactory 与 ApplicationContext 的关系

（1）`BeanFactory`是`Spring`的早期接口，称为`Spring`的`Bean`工厂，`ApplicationContext`是后期更高级的接口，称之为`Spring`容器；
（2）`ApplicationContext`在`BeanFactory`基础上对功能进行了拓展，例如：监听功能、国际化功能等。`BeanFactory`的`API`更偏向于底层，`ApplicationContext`的`API`大多数是对这些底层`API`的封装；
（3）`Bean`创建的主要逻辑和功能都被封装在`BeanFactory`中，`ApplicationContext`不仅继承了`BeanFactory`，而且`ApplicationContext`内部还维护着`BeanFactory`的引用，所以，`ApplicationContext`与`BeanFactory`既有继承关系，又有融合关系；
（4）`Bean`的初始化时机不同，原始`BeanFactory`是在首次调用`getBean`时才进行`Bean`的创建，而`ApplicationContext`则时配置文件加载，容器一创建就将`Bean`都实例化并初始化好。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171422292.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171427148.png)

### 1.3.5 ApplicationContext 的继承体系

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171440412.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171442132.png)

## 1.4 基于 xml 的 Spring 应用

### 1.4.1 Spring Bean 的配置详解

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171506874.png)
（1）Bean 的基本配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171516167.png)
（2）Bean 的别名配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171523768.png)
（3）Bean 的作用范围的配置
默认情况下，单纯的 Spring 环境 Bean 的作用范围有两个：`Singleton`和`Prototype`

- Singleton：单例，默认值，`Spring`容器创建的时候，就会进行`Bean`的实例化，并存储到容器内部的单例池中，每次`getBean`时都是从单例池中获取相同的`Bean`实例；
- prototype：原型，`Spring`容器初始化时不会创建`Bean`实例，当调用`getBean`时才会实例化`Bean`，每次`getBean`都会创建一个新的`Bean`实例。
  （4）Bean 的延迟加载
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171628100.png)
  （5）Bean 的初始化方法和销毁方法配置
  `Bean`在被实例化后，可以执行指定的初始化方法完成一些初始化的操作，`Bean`在销毁之前也可以执行指定的销毁方法完成一些操作，初始化方法名称和销毁方法名称通过
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171632418.png)
  InitializingBean 接口
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171642428.png)
  （6）Bean 实例化的方法
  `Spring`实例化方式主要有两种：

  - 构造方式实例化：底层通过构造方法对`Bean`进行实例化
  - 工厂方式实例化：底层通过调用自定义的工厂方法对`Bean`进行实例化

  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308171654475.png)

  工厂方式实例化 Bean，又分为如下三种：

  - 静态工厂方法实例化 Bean
  - 实例工厂方法实例化 Bean
  - 实现 FactoryBean 规范延迟实例化 Bean
