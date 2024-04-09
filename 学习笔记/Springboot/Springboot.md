
>Springboot以**约定大于配置的核心思想**

高内聚，低耦合。

>Springboot自动装配原理：
     
<img src="F:\java\IDEA Projects\JavaSE_Code\学习\SpringBoot\Springboot自动分配.jpg"/>


> 结论： 

springboot所有自动配置都是在启动的时候扫描并加载：`spring.factories` 所有的自动配置类都在这里面，但是不一定生效，
要判断条件是否成立，只要导入了对应的start，就有对应的启动器了，有了启动器，我们自动装配就会生效，然后就配置成功！
>步骤：

1. SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值

2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；

3. 整个JavaEE的整体解决方案和自动配置都在springboot-autoconfigure的jar包中；

4. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器；

5. 它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration的文件@Bean）, 就是给容器中导入这个场景需要的所有组件 ，并配置好这些组件（@Configuration, javaConfig）；

6. **有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；**

> 关于SpringBoot,谈谈你的理解：
- 自动装配
- run()；
 
    1. 推断应用的类型是普通的项目还是Web项目

    2. 查找并加载所有可用初始化器 ， 设置到initializers属性中

    3. 找出所有的应用程序监听器，设置到listeners属性中

    4. 推断并设置main方法的定义类，找到运行的主类 


> yaml 文件编写规则
  
1. 普通的key-value
2. 对空格的要求身分高
3. 可以注入到我们配置中（可以给我们的实体类赋值）

    name: jiapb

> 对象

student:

name: jiapb

age: 3

>行内写法（集合）

`student: {name: jiapb,age: 3}`

> 数组
  
pets:
  - cat
  - dog
  - pig

`pets: [cat,dog,pig]`


#properties 只能保存键值对！

#### **JSR303数据校验**

>空检查

@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.

>Booelan检查

@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false

>长度检查

@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

>日期检查

@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则

>Springboot 自动装配原理 （精髓）

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

xxxxAutoConfigurartion：自动配置类；给容器中添加组件

xxxxProperties:封装配置文件中相关属性；

>了解： @Conditional

了解完自动装配的原理后，我们来关注一个细节问题，自动配置类必须在一定的条件下才能生效；

@Conditional派生注解（Spring注解版原生的@Conditional作用）

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；
![img.png](img.png)
**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**

我们怎么知道哪些自动配置类生效？

我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；

`#开启springboot的调试类`

`debug=true`

Positive matches:（自动配置类启用的：正匹配）

Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）

Unconditional classes: （没有条件的类）

>Springboot整合JDBC

    

