# 4.1 简介

Swagger 是一套基于 OpenAPI 规范构建的开源工具，可以帮助我们设计、构建、记录以及使用 Rest API。 

# 4.2 为什么使用Swagger

当下很多公司都采取前后端分离的开发模式，前端和后端的工作由不同的工程师完成。在这种开发模式下，维持一份及时更新且完整的 Rest API 文档将会极大的提高我们的工作效率。传统意义上的文档都是后端开发人员手动编写的，相信大家也都知道这种方式很难保证文档的及时性，这种文档久而久之也就会失去其参考意义，反而还会加大我们的沟通成本。而 Swagger 给我们提供了一个全新的维护 API 文档的方式，下面我们就来了解一下它的优点： 

1. 代码变，文档变。只需要少量的注解，Swagger 就可以根据代码自动生成 API 文档，很好的保证了文档的时效性。 2. 跨语言性，支持 40 多种语言。 3. Swagger UI 呈现出来的是一份可交互式的 API 文档，我们可以直接在文档页面尝试 API 的调用，省去了准备复杂的调用参数的过程。


# 4.3 快速入门
## 4.3.1 引入依赖
```xml
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
        </dependency>
```

## 4.3.2 启用 Swagger2

在启动类上或者配置类加 @EnableSwagger2 注解
```java
@SpringBootApplication
@MapperScan("com.sangeng.mapper")
@EnableScheduling
@EnableSwagger2
public class SanGengBlogApplication {
    public static void main(String[] args) {
        SpringApplication.run(SanGengBlogApplication.class,args);
    }
}
```

## 4.3.3 测试

访问：[http://localhost:7777/swagger-ui.html](http://localhost:7777/swagger-ui.html) 注意其中 localhost 和 7777 要调整成实际项目的域名和端口号。

# 4.4 具体配置
### 4.4.1 Controller 配置
### 4.4.1 @Api 注解
属性介绍：
tags 设置标签
description 设置描述信息，过时也是可以使用的
```java
@RestController
@RequestMapping("/comment")
@Api(tags = "评论",description = "评论相关接口")
public class CommentController {
}
```

### 4.4.2 接口配置
#### 4.4.2.1 接口描述配置@ApiOperation
Value 设置接口描述信息
notes 设置补充信息
```java
    @GetMapping("/linkCommentList")
    @ApiOperation(value = "友链评论列表",notes = "获取一页友链评论")
    public ResponseResult linkCommentList(Integer pageNum,Integer pageSize){
        return commentService.commentList(SystemConstants.LINK_COMMENT,null,pageNum,pageSize);
    }
```

#### 4.4.2.2 接口参数描述
@ApiImplicitParam 用于描述接口的参数，但是一个接口可能有多个参数，所以一般与  @ApiImplicitParams 组合使用。 
```java
@GetMapping("/commentList")  
@ApiOperation(value = "友链评论列表",notes = "获得一页友链评论")  
@ApiImplicitParams({  
        @ApiImplicitParam(name = "pageNumber", value = "页码"),  
        @ApiImplicitParam(name = "pageSize", value = "页数大小")  
  
})  
public ResponseResult commentList(Long articledId, Integer pageNumber, Integer pageSize) {  
    return commentService.commentList(SystemConstants.LINK_COMMENT,articledId, pageNumber, pageSize);  
}
```

### 4.4.3 实体类配置
#### 4.4.3.1 实体的描述配置@ApiModel
@ApiModel 用于描述实体类。
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ApiModel(description = "添加评论dto")
public class AddCommentDto{
    //..
}
```

#### 4.4.3.2 实体的属性的描述配置@ApiModelProperty
@ApiModelProperty 用于描述实体的属性
```java
    @ApiModelProperty(notes = "评论类型（0代表文章评论，1代表友链评论）")
    private String type;
```

### 4.4.4 文档信息配置
创建配置类
```java
@Configuration
public class SwaggerConfig {
    @Bean
    public Docket customDocket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.sangeng.controller"))
                .build();
    }

    private ApiInfo apiInfo() {
        Contact contact = new Contact("is", "http://www.zml.com", "my@my.com");
        return new ApiInfoBuilder()
                .title("博客前台接口")
                .description("博客前台")
                .contact(contact)   // 联系方式
                .version("1.1.0")  // 版本
                .build();
    }
}
```





