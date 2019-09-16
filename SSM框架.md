<!-- TOC -->

- [1 Spring常用注解](#1-spring常用注解)
- [2 JavaBean注入方式](#2-javabean注入方式)
    - [2.1 Set()注入](#21-set注入)
    - [2.2 构造器注入](#22-构造器注入)
    - [2.3 注解注入](#23-注解注入)
- [3 JavaBean的声明周期](#3-javabean的声明周期)
- [4 项目打包过程](#4-项目打包过程)
- [5 Mybatis配置](#5-mybatis配置)
- [6 数据库连接池参数](#6-数据库连接池参数)
- [7 spring事务管理](#7-spring事务管理)

<!-- /TOC -->
# 1 Spring常用注解
* @AutoWired：自动装配
其作用是消除Java代码中的getter/setter与bean属性中的property。@Autowired默认按类型匹配的方式，在容器中查找匹配的bean。需要引入如下的xml配置：
```xml
<context:component-scan base-package="XXXX"/>
```
* @Qualifier：指定注入Bean的名称，如果容器中有一个以上匹配的Bean，则可以通过@Qualifier注解限定Bean的名称;
* @Component：是所有受spring管理组件的通用形式，可以放在类的头上，不推荐使用；
* @Controller：对应表现层的Bean，控制层；
* @Service：对应业务层的Bean；
* @Repository：对应Dao层数据访问层的Bean;
* @Scope：注解作用域；

# 2 JavaBean注入方式
## 2.1 Set()注入
假设有一个SpringAction类，类里需要实例化一个SpringDao对象，那么可以定义一个私有的SpringDao成员变量，然后创建SpringDao的set方法（IOC的注入入口）：
```java
package com.bless.springdemo.action;
public class SpringAction{
    private SpringDao springDao;
    public void setSpringDao(SpringDao springDao){
        this.springDao = springDao;
    }
}
```
在随后编写的xml文件中
```xml
<!--配置bean,配置后该类由spring管理--> 
<bean name="springAction" class="com.bless.springdemo.action.SpringAction"> 
<!--(1)依赖注入,配置当前类中相应的属性--> 
<property name="springDao" ref="springDao"></property> 
</bean> 
<bean name="springDao" class="com.bless.springdemo.dao.impl.SpringDaoImpl"></bean>
```
## 2.2 构造器注入
这种方法是指有参数的构造函数注入，如下
```java
public class SpringAction{
    private SpringDao springDao;
    private User user;
    public SpringAction(SpringDao springDao,User user){
        this.springDao = springDao;
        this.user = user;
    }
}
```
在XML文件中同样不用的形式，而是使用标签，ref属性同样指向其它标签的name属性：
```xml
<!--配置bean,配置后该类由spring管理--> 
<bean name="springAction" class="com.bless.springdemo.action.SpringAction"> 
<!--(2)创建构造器注入,如果主类有带参的构造方法则需添加此配置--> 
<constructor-arg index="0" ref="springDao"></constructor-arg>
<constructor-arg index="1" ref="user"></constructor-arg>
</bean> 
<bean name="springDao" class="com.bless.springdemo.dao.impl.SpringDaoImpl"></bean> 
<bean name="user" class="com.bless.springdemo.vo.User"></bean> 
```
解决构造方法参数的不确定性：你可能会遇到构造方法传入的两参数都是同类型的，为了分清哪个该赋对应值，使用index。
## 2.3 注解注入
四种主要注解
@Component：主要用于注册所有Bean；
@Repository：主要用于注册Dao层的Bean；
@Controller：主要用于注册控制层的Bean；
@Service：主要用于注册服务层的Bean.
@Scope()："prototype"（原型模式），"singleton"（单例模式）设置JavaBean的作用范围； 
Dao层接口UserDao.java
```java
public interface UserDao{
    public int save();
}
```
UserDaoImpl.java
```java
@Repository("UserDao")// 这个注解等同于在XML配置文件中编写<bean id="userDao" class="..UserDaoImp"/>
public class UserDaoImpl implements UserDao{
    public int save(){
        return 1;
    }
} 
```
在需要注入的地方，服务层实现类
```java
public class UserServiceImpl implements UserService{
    // 声明接口类型的引用,和具体实现类解耦合
    @Autowired //默认按类型匹配
    @Qualifier("userDao") // 指定Bean名称匹配
    private UserDao dao;
}
```
# 3 JavaBean的声明周期

# 4 项目打包过程
* IDEA -> File  -> Project Structure
* Project Settings -> Artifacts -> + -> Web Application: Exploded（在output目录中生成文件夹）
* Project Settings -> Artifacts -> + -> Web Application: Archive（选择上一步生成的文件夹名称，会在输入目录生成.war包）
* 将文件夹或war包拷贝到tomcat的wabapps目录下，然后在浏览器输入localhost:端口/文件名称
# 5 Mybatis配置

# 6 数据库连接池参数
* initialSize 初始化连接数目 
* maxActive 最大连接数
* maxWait 连接池中连接用完时，新的请求等待时间（毫秒）
* removeAbandonedTimeout 活动连接的最大空闲时间,单位为秒 超过此时间的连接会被释放到连接池中,针对未被close的活动连接  

# 7 spring事务管理
