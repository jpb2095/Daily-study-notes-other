# **Swagger**
>1.配置Swagger
    （springboot版本是2.6.17,swagger是3.0版本）
 1. 添加依赖

    ```
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-boot-starter</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```
 2. 配置文件里添加注解来兼容

    application.yml 或applicaiton.properties 中添 必须 加如下配置
    `spring.mvc.pathmatch.matching-strategy=ant-path-matcher`或
    ```
    spring:
         mvc:
             pathmatch:
                 matching-strategy: ant_path_matcher
    ```
 3. 在只需要在启动类上加 @EnableSwagger2注解即可(也可以是@EnableOpenApi，同样的效果，但两者区别我目前没搞懂)
    访问地址：http://localhost:8080/swagger-ui/index.html
    


>实体类配置 
```
    //@Api(注释)
    @ApiModel("用户实体类")
    public class User {
    @ApiModelProperty("用户名")
    public String username;
    @ApiModelProperty("密码")
    public String password;
}
```

>controller配置
```
//API给类上注释
@Api(tags = ("hello控制器"),description = "hello控制器")
@RestController
public class HelloController {

    @GetMapping(value = "/hello")
     public String hello1(){
        return "hello";
     }

     //只要我们的接口中，返回值中存在实体类，他就会被扫描到Swagger中
    @PostMapping(value = "/USER")
    public User user(){
        return new User();
    }
    
    //Operation接口，不是放在类上，而是方法上（都是注释）
    @ApiOperation("Hello控制方法")
    @GetMapping(value = "/hello2")
    public String hello2(@ApiParam("用户名") String username){
        return "hello"+ username;
    }
    
    @ApiOperation("Post测试")
    @PostMapping(value = "/postt")
    public User postt(@ApiParam("用户") User user){
        return  user;
    }
}
```

@ApiModel为类添加注释

@ApiModelProperty为类属性添加注释


>常用注解：

Swagger的所有注解定义在io.swagger.annotations包下

下面列一些经常用到的，未列举出来的可以另行查阅说明：

|                     Swagger注解                      |              	 简单说明               |
|:--------------------------------------------------:|:---------------------------------:|
|              @Api(tags = "xxx模块说明")	               |              作用在模块类上              |
|             @ApiOperation("xxx接口说明")	              |             作用在接口方法上              |  
|              @ApiModel("xxxPOJO说明")	               |          作用在模型类上：如VO、BO           |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) |  作用在类方法和属性上，hidden设置为true可以隐藏该属性  |
|               @ApiParam("xxx参数说明")	                | 作用在参数、方法和字段上，类似@ApiModelProperty  |

>总结：

1. 我们可以通过Swagger给一些比较难理解的属性或者接口，增加注释信息
2. 接口文档实时更新
3. 可以在线测试
Swagger是一个优秀的工具，几乎所有大公司都有使用它
【注意点】 在正式发布的时候，关闭Swagger!!!出于安全考虑。而且节省运行的内存;
