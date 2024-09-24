---
layout: post
title: Express框架学习
date: 2024-06-13 10:20:26
tags:
- NodeJS
category:
- 后端学习
---
## 1.创建Express项目
创建`express`项目可以通过使用`express-generator`脚手架，通过它，一个命令就会自动生成项目所需要的结构了。
  ```bash
  # 安装
  npm i -g express-generator@4

  # 创建项目（到指定文件夹下）
  express --no-view 项目名称
  ```
`--no-view`参数的意思是不需要任何视图模板，适用于专门做后端接口的项目创建。
```bash
  # 安装依赖
  npm i
  # 运行
  npm start
```
现在就可以通过 `http://localhost:3000`，来访问项目了。

> 现在对项目中的内容每次修改之后，都需要重启项目来查看，那么是否有更好的方式能够实现热修改呢？

## 2.nodemon监听修改
```bash
# 安装
npm i nodemon
```
然后打开项目根目录下的`package.json`，将`start`这里修改为
```json
"scripts": {
  "start": "nodemon ./bin/www"
},
```
再次重启服务即可使用

## 3.项目结构
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202406131042072.png)

+ `bin/www`：在`package.json`中，大家见过这个文件的配置。它是用来启动项目的文件，无需修改，也不用管它，知道它是干嘛的就好了。
+ `public 目录`：这里放的各种静态资源，例如 CSS、图片等等静态资源。但因为我们项目是专门开发接口的，所以这里的东西，大家完全不需要管它，根本用不上。
+ `routes`：程序的路由部分，路由简单的理解就是将不同网址分别对应到不同的程序代码上去。
+ `app.js`：这个文件也很重要，在开发中，我们需要做一些路由的配置、跨域配置，都会来修改它的。

## 4.使用 Docker 运行 MySQL
**docker compose**
在项目根目录中，新建一个文件，叫做docker-compose.yml。千万要注意，一定要在项目根目录中，放在其他地方会找不到的。然后将下面的配置复制进去，这就是MySQL的一个简单配置了。
```yml
services:
  mysql:
    image: mysql:8.3.0
    command:
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
    environment:
      - MYSQL_ROOT_PASSWORD=MySQL的密码
      - MYSQL_LOWER_CASE_TABLE_NAMES=0
    ports:
      - "3307:3306" # 端口映射：本机端口:容器端口
    volumes:
      - ./data/mysql:/var/lib/mysql

```
运行`docker-compose up -d`下载mysql，完成后到Docker中启动数据库服务，在本机可以通过`Navicat`，`http://localhost:3307`进行访问。

## 5.Sequelize ORM
Sequelize 是一个用于 Node.js 的流行的对象关系映射（ORM）库。

它主要有以下特点和作用：

- **简化数据库操作**：通过将数据库表和操作映射到 JavaScript 对象和方法，使开发者可以使用面向对象的方式与数据库进行交互，而无需直接编写复杂的 SQL 语句。
- **提供模型定义**：可以方便地定义数据库中的模型（表结构），包括字段、关系等。
- **数据操作接口**：提供了诸如创建、读取、更新、删除（CRUD）等操作的方法，以及处理关联模型的能力。
- **数据库连接管理**：帮助管理与数据库的连接和相关配置。
- **可移植性**：使得应用在不同的数据库系统（如 MySQL、PostgreSQL、SQLite 等）之间切换相对容易，只需更改少量配置。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202406131105775.png)

### 5.1 Sequelize ORM 的使用
```bash
# 安装
npm i -g sequelize-cli
```
在当前项目目录下安装所依赖的sequelize包和对数据库支持依赖的mysql2
```bash
npm i sequelize mysql2
```
初始化项目
```bash
sequelize init
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202406131115578.png)
可以看到，提示我们，创建了一个config配置文件和三个目录，这些就是sequelize所需要的东西了。

```
.
├── config
│   └── config.json
├── migrations
├── models
│   └── index.js
└── seeders
```
+ config.json：sequelize连接数据库的配置文件，配置数据库连接需要的相关信息
+ migrations：是迁移的意思，如果你需要对数据库做新增表、修改字段、删除表等等操作，就需要在这里添加迁移文件了。而不是像以前那样，使用客户端软件来直接操作数据库。
+ models：这里面存放的是模型文件，当我们使用sequelize来执行增删改查时，就需要用这里的模型文件了。每个模型都对应数据库中的一张表。
+ seeders，是存放的种子文件。一般会将一些需要添加到数据表的测试数据存在这里。只需要运行一个命令，数据表中就会自动填充进一些用来测试内容的了。

`config.json`文件的内容如下：
```json
{
  "development": {
    "username": "root",
    "password": "clwy1234",
    "database": "clwy_api_development",
    "host": "127.0.0.1",
    "dialect": "mysql",
    "timezone": "+08:00"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "clwy_api_test",
    "host": "127.0.0.1",
    "dialect": "mysql",
    "timezone": "+08:00"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "clwy_api_production",
    "host": "127.0.0.1",
    "dialect": "mysql",
    "timezone": "+08:00"
  }
}
```
### 5.2创建模型
```bash
sequelize model:generate --name Article --attributes title:string,content:text
```
打开`models/article.js`。可以看到，在模型文件夹中，出现了一个叫做`Article`的模型，它里面有标题和内容。

标题是字符串类型，对应到 `MySQL` 数据库里，它就会自动变成`varchar`。内容部分，则是`text`类型。

### 5.3迁移文件
在`migrations`文件夹，里面出现了一个由当前时间，加上`create-article`命名的文件，这个文件就是迁移文件了。<i style="color:red;">它的作用就是用来创建、修改表的。</i>

看到这里，在up部分。我们通过createTabel，创建了一个叫做Articles的表。

<i style="color:red;">大家注意哦，这就是我之前和大家说的，模型名字是单数：Article。但是表的名字一定是复数形式，也就是Articles。</i>这是sequelize里的命名规则，一定不要搞错了。如果你的单复数搞错了，那sequelize就找不到对应的东西了。

接着往下看，这些就是定义了Articles这张表里面所拥有的字段了，比方说id、title、content，这些外还出现了两个时间字段createdAt和updatedAt。

这两个字段，当在新增或修改数据的时候，sequelize会自动的帮我们填写的。

这里的内容，已经非常好了，我们只需要做一个小小的调整。就是title部分，我需要它不为null，所以加上allowNull: false,。content部分，我们就不管它了，已经很好了。

至于down部分，是新建表的反向操作，里面写的是dropTable，也就是删除当前的表。这样当我们创建表，建完后，突然又发现有错误，也可以通过相关命令来删除当前表。

运行迁移文件：`sequelize db:migrate`

打开数据库客户端，刷新一下，可以看到Articles表又神奇的出现了。看一看结构选项卡，里面的字段和我们当时自己手动创建的完全一样，而且还多了两个时间字段。这就是迁移文件的作用了。

### 5.4 种子文件
现在表也有了，下一步就是要填充一些在开发中用来测试的数据了。当然你可以用手动往里面一点点填，但很多情况我们做测试，可能需要非常多的数据。例如我希望数据库里有 100 篇文章，这时候，我们一条条的录入也太笨了点。最简单的方法就是使用种子文件了。再来试试这条命令
```zsh
sequelize seed:generate --name article
```
完成后，在`seeds`目录，就看到刚才命令新建的种子文件了。同样也是分为两个部分，`up`部分用来填充数据，`down`部分是反向操作，用来删除数据的。

通过`up`方法往`Articles`表里插入数据。
```js
async up (queryInterface, Sequelize) {
  const articles = [];
  const counts = 100;

  for (let i = 1; i <= counts; i++) {
    const article = {
      title: `文章的标题 ${i}`,
      content: `文章的内容 ${i}`,
      createdAt: new Date(),
      updatedAt: new Date(),
    };

    articles.push(article);
  }

  await queryInterface.bulkInsert('Articles', articles, {});
},
```
`down`部分进行修改
```js
async down (queryInterface, Sequelize) {
  await queryInterface.bulkDelete('Articles', null, {});
}
```
执行种子文件，往数据库中插入数据
```bash
sequelize db:seed --seed seeds目录下的js文件名字（如xxx-article）
```

## 6.CRDU接口
### 6.1 处理路由文件
项目的路由文件存放在`routes`文件夹下，对于每一类待写的接口，都要在`routes`文件夹下创建新的js文件。

```javascript
var express = require("express");
var router = express.Router();

/* GET home page. */
router.get("/", function (req, res, next) {
  res.json({ message: "Hello express1.0" });
});

module.exports = router;
```
### 6.2 app.js
在`app.js`文件中引入创建的路由文件。
例如：
```javascript
var articlesRouter = require("./routes/admin/articles");

app.use("/admin/articles", articlesRouter);
```
其中，`/admin/articles`就是新创建的路由的地址。例如：`http://localhost:3000/admin/articles`可以访问其中的接口。

### 6.3处理接口返回信息
对于接口返回信息而言，分为请求成功和请求失败两种情况。对于这两种情况，一般需要返回状态码、数据、信息等，因此可以将其抽取出来封装成为公共方法。
一般将这类方法存在根目录下的`utils`文件夹中。
```javascript
// response.js 文件
/**
 * 自定义 404 错误类
 */
class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = "NotFoundError";
  }
}

function success(res, message, data = {}, code = 200) {
  res.status(code).json({
    status: true,
    message,
    data,
  });
}

function failure(res, error) {
  if (error.name === "SequelizeValidationError") {
    const errors = error.errors.map((e) => e.message);
    res.status(400).json({
      status: false,
      message: "请求参数错误",
      errors,
    });
  }
  if (error.name === "NotFoundError") {
    return res.status(404).json({
      status: false,
      message: "资源不存在",
      errors: [error.message],
    });
  }
  res.status(500).json({
    status: false,
    message: "服务器错误",
    errors: [error.message],
  });
}

module.exports = {
  NotFoundError,
  success,
  failure,
};

```
### 6.4 接口
对于接口方法中的通用功能，可以将其抽取成为一个公共方法。
例如，根据id号查询具体文章方法在多个借口中都会使用到。
```javascript
/**
 * 公共方法：查询当前文章
 */
async function getArticle(req) {
  // 获取文章id
  const { id } = req.params;
  // 查询当前文章
  const article = await Article.findByPk(id);
  if (!article) {
    throw new NotFoundError(`ID:${id}的文章未找到`);
  }
  return article;
}

// 白名单过滤（强参数过滤）：保证只使用到用户传递过来具体的字段，防止用户传递有害信息
function filterBody(req) {
  return {
    title: req.body.title,
    content: req.body.content,
  };
}
```
```javascript
// 增：创建文章接口
router.post("/", async (req, res) => {
  try {
    const body = filterBody(req);
    const article = await Article.create(body);
    success(res, "创建文章成功", { article }, 201);
  } catch (error) {
    failure(res, error);
  }
});

// 删：删除文章接口
router.delete("/:id", async (req, res) => {
  try {
    const article = await getArticle(req);
    await article.destroy();
    success(res, "删除文章成功");
  } catch (error) {
    failure(res, error);
  }
});

// 改：更新文章接口
router.put("/:id", async (req, res) => {
  try {
    const article = await getArticle(req);
    const body = filterBody(req);
    await article.update(body);
    success(res, "更新文章成功", { article });
  } catch (err) {
    failure(res, err);
  }
});

// 查：查询文章数据
// 1.查询文章列表（分页 & 条件搜索）
router.get("/", async function (req, res, next) {
  try {
    const query = req.query;
    const condition = {
      order: [["id", "DESC"]],
    };
    if (query.title) {
      // 通过Op的方式实现模糊查询
      condition.where = {
        title: {
          [Op.like]: `%${query.title}%`,
        },
      };
    }
    // 数据分页
    // 当前是第几页，如果不传，那就是第一页
    const currentPage = Math.abs(Number(query.currentPage)) || 1;
    // 每页显示多少条信息，如果不传，就显示10条信息
    const pageSize = Math.abs(Number(query.pageSize)) || 10;
    // 计算offset
    const offset = (currentPage - 1) * pageSize;
    condition.limit = pageSize;
    condition.offset = offset;
    // const articles = await Article.findAll(condition);
    // 分页更换查询函数findAndCountAll，rows是查询到的数据，count是数据总条数
    const { count, rows } = await Article.findAndCountAll(condition);
    const data = {
        articles: rows,
        pagination: {
          total: count,
          currentPage,
          pageSize,
        },
      };
    success(res,"查询文章列表成功",data);
  } catch (err) {
    failure(res, err);
  }
});
```
## 7.企业内项目开发的流程
1. 编写需求文档
2. 原型与UI设计
3. 确定数据库的表、字段以及接口地址和数据
4. 同时进行：前端mock开发、后端接口开发
5. 接口开发完成后，将mock地址更换为接口地址
6. 测试、上线部署

## 8.建立数据库表
回滚迁移：`sequelize db:migrate:undo`，运行命令后，会回滚上一次运行的迁移，也就是删掉`Article`表。
以创建`Courses`表为例：

1. 运行创建命令：`sequelize model:generate --name Course --attributes categoryId:integer,userId:integer,name:string,image:string,recommended:boolean,introductory:boolean,content:text,likesCount:integer,chaptersCount:integer`
2. 修改`migrations`文件夹下的对应`course`文件：
```javascript
"use strict";
const { toDefaultValue } = require("sequelize/lib/utils");

/** @type {import('sequelize-cli').Migration} */
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable("Courses", {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER.UNSIGNED,
      },
      categoryId: {
        allowNull: false,
        type: Sequelize.INTEGER.UNSIGNED,
      },
      userId: {
        allowNull: false,
        type: Sequelize.INTEGER.UNSIGNED,
      },
      name: {
        allowNull: false,
        type: Sequelize.STRING,
      },
      image: {
        type: Sequelize.STRING,
      },
      recommended: {
        type: Sequelize.BOOLEAN,
      },
      introductory: {
        type: Sequelize.BOOLEAN,
      },
      content: {
        type: Sequelize.TEXT,
      },
      likesCount: {
        allowNull: false,
        defaultValue: 0,
        type: Sequelize.INTEGER.UNSIGNED,
      },
      chaptersCount: {
        allowNull: false,
        defaultValue: 0,
        type: Sequelize.INTEGER.UNSIGNED,
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE,
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE,
      },
    });
    await queryInterface.addIndex("Courses", {
      fields: ["categoryId"],
    });
    await queryInterface.addIndex("Courses", {
      fields: ["userId"],
    });
  },
  async down(queryInterface, Sequelize) {
    await queryInterface.dropTable("Courses");
  },
};

```



