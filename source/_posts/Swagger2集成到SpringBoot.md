---
title: Swagger2集成到SpringBoot
date: 2023-10-26 10:15:42
tags:
  - java
---

`swagger`是当下比较流行的实时接口文文档生成工具。接口文档是当前前后端分离项目中必不可少的工具，在前后端开发之前，后端要先出接口文档，前端根据接口文档来进行项目的开发，双方开发结束后在进行联调测试。

## SpringBoot 集成 swagger2

### 1.引入依赖

```xml
// 在pom.xml文件中加入swagger2使用的依赖
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>

```

### 2.swagger 配置

swagger 的使用需要对其进行配置，一般是在项目的根目录下配置`SwaggerConfig`文件【`config`包下的`SwaggerConfig`】

```java
@Configuration //告诉Spring容器，这个类是一个配置类
@EnableSwagger2//启用Swagger2功能
public class SwaggerConfig {
    /**
     * 配置swagger2相关的bean
     */
    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com"))//com包下的所有API都交给Swagger2管理
                .paths(PathSelectors.any()).build();
    }
    /**
     * 此处主要是API文档页面显示信息
     */
    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("演示项目API")
                .description("演示项目")
                .version("1.0")
                .build();
    }
}

```

### 3.其他配置

此时启动项目，项目会报错（SpringBoot 版本：`2.7.17`）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202310261025523.png)
出错原因：SpringBoot 版本与 Swagger 版本不匹配导致。Spring Boot 2.6.X 及以上使用 PathPatternMatcher 匹配路径，Swagger 引用的 Springfox 使用的路径匹配是基于 AntPathMatcher 的。
解决方法：
在 springBoot 配置文件中添加配置：
`spring.mvc.pathmatch.matching-strategy=ANT_PATH_MATCHER`

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202310261028695.png)
再次启动问题解决。

访问地址： ip:端口号/swagger-ui.html

问题解决！！！

### 4.使用示例

```java
// UserController文件
@RestController
@Api(value = "用户接口",tags={"用户接口"})
public class UserController {
    @GetMapping("/user/{id}")
    @ApiOperation("根据ID查询用户")
    public String getUserById(@PathVariable int id){
        return "根据ID获取用户";
    }
    @PostMapping("/user")
    @ApiOperation("添加用户")
    public String save(User user){
        return "添加用户";
    }
    @PutMapping("/user")
    @ApiOperation("修改用户")
    public String update(User user){
        return "更新用户";
    }
    @DeleteMapping("/user/{id}")
    @ApiOperation("根据ID删除用户")
    public String deleteById(@PathVariable int id){
        return "根据ID删除用户";
    }
}
```
