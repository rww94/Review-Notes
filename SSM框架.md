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
- [8 大规模数据的实时计算](#8-大规模数据的实时计算)
- [9 Nginx](#9-nginx)
    - [9.1 Nginx的特点](#91-nginx的特点)
    - [9.2 前端使用Nginx](#92-前端使用nginx)
    - [9.3 Nginx命令](#93-nginx命令)
    - [9.4 Nginx基本配置](#94-nginx基本配置)

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

# 8 大规模数据的实时计算
数据量进一步上升，需要的网络吞吐、计算能力骤增，后端的算力难以跟上数据量的增长。这时候可以将计算能力分散到全链路，将计算提前到数据产生端。例如在前端进行数据的实时计算然后存储到本地缓存中，需要有机制维持缓存和数据库的一致性。

# 9 Nginx
[参考博客](https://blog.csdn.net/finnson/article/details/82461600)
## 9.1 Nginx的特点
* 核心特点：高并发请求的同时保持高效的服务
* 热部署
* 低内存消耗
* 处理响应请求很快
* 具有很高的可靠性
* 同时Nginx可以实现高效的反向代理和均衡负载
## 9.2 前端使用Nginx
* 搭建静态资源服务器
* 反向代理分发后端服务（可以和node.js搭配实现前后端分离）和跨域问题
* 根据User Agent来重定向站点
* 开发环境和测试环境切换（切换host）
* URL重写，使用review规则本地映射
* 资源内容篡改
* 获取cookie做分流
* 资源合并
* 压缩图片
## 9.3 Nginx命令
* start nginx 开启
* nginx -s reload 重启

## 9.4 Nginx基本配置
* main 全局设置，影响其他部分的所有设置
* server 主机服务相关设置，主要用于指定虚拟机域名、IP和端口
* location URL匹配特定位置后的设置，反向代理、内容篡改相关设置
* upstream 上游服务器设置，均衡负载相关配置
<details>
<summary>下面是一份基本配置</summary>

```
#定义 Nginx 运行的用户和用户组,默认由 nobody 账号运行, windows 下面可以注释掉。 
user  nobody; 
 
#nginx进程数，建议设置为等于CPU总核心数。可以和worker_cpu_affinity配合
worker_processes  1; 
 
#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
 
#进程文件，window下可以注释掉
#pid        logs/nginx.pid;
 
# 一个nginx进程打开的最多文件描述符(句柄)数目，理论值应该是最多打开文件数（系统的值ulimit -n）与nginx进程数相除，
# 但是nginx分配请求并不均匀，所以建议与ulimit -n的值保持一致。
worker_rlimit_nofile 65535;
 
#工作模式与连接数上限
events {
    # 参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; 
    # epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
   #use epoll;
   #connections 20000;  # 每个进程允许的最多连接数
   # 单个进程最大连接数（最大连接数=连接数*进程数）该值受系统进程最大打开文件数限制，需要使用命令ulimit -n 查看当前设置
   worker_connections 65535;
}
 
#设定http服务器
http {
    #文件扩展名与文件类型映射表
    #include 是个主模块指令，可以将配置文件拆分并引用，可以减少主配置文件的复杂度
    include       mime.types;
    #默认文件类型
    default_type  application/octet-stream;
    #charset utf-8; #默认编码
 
    #定义虚拟主机日志的格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    
    #定义虚拟主机访问日志
    #access_log  logs/access.log  main;
 
    #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
    sendfile        on;
    #autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。
 
    #防止网络阻塞
    #tcp_nopush     on;
 
    #长连接超时时间，单位是秒，默认为0
    keepalive_timeout  65;
 
    # gzip压缩功能设置
    gzip on; #开启gzip压缩输出
    gzip_min_length 1k; #最小压缩文件大小
    gzip_buffers    4 16k; #压缩缓冲区
    gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_comp_level 6; #压缩等级
    #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
    gzip_vary on; //和http头有关系，加个vary头，给代理服务器用的，有的浏览器支持压缩，有的不支持，所以避免浪费不支持的也压缩，所以根据客户端的HTTP头来判断，是否需要压缩
    #limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用
 
    # http_proxy服务全局设置
    client_max_body_size   10m;
    client_body_buffer_size   128k;
    proxy_connect_timeout   75;
    proxy_send_timeout   75;
    proxy_read_timeout   75;
    proxy_buffer_size   4k;
    proxy_buffers   4 32k;
    proxy_busy_buffers_size   64k;
    proxy_temp_file_write_size  64k;
    proxy_temp_path   /usr/local/nginx/proxy_temp 1 2;
 
   # 设定负载均衡后台服务器列表 
    upstream  backend.com  { 
        #ip_hash; # 指定支持的调度算法
        # upstream 的负载均衡，weight 是权重，可以根据机器配置定义权重。weigth 参数表示权值，权值越高被分配到的几率越大。
        server   192.168.10.100:8080 max_fails=2 fail_timeout=30s ;  
        server   192.168.10.101:8080 max_fails=2 fail_timeout=30s ;  
    }
 
    #虚拟主机的配置
    server {
        #监听端口
        listen       80;
        #域名可以有多个，用空格隔开
        server_name  localhost fontend.com;
        # Server Side Include，通常称为服务器端嵌入
        #ssi on;
        #默认编码
        #charset utf-8;
        #定义本虚拟主机的访问日志
        #access_log  logs/host.access.log  main;
        
        # 因为所有的地址都以 / 开头，所以这条规则将匹配到所有请求
        location / {
            root   html;
            index  index.html index.htm;
        }
        
        #error_page  404              /404.html;
 
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
 
       # 图片缓存时间设置
       location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$ {
          expires 10d;
       }
 
       # JS和CSS缓存时间设置
       location ~ .*.(js|css)?$ {
          expires 1h;
       }
 
        #代理配置
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #location /proxy/ {
        #    proxy_pass   http://127.0.0.1;
        #}
 
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
 
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
 
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;
 
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
 
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;
 
    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;
 
    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;
 
    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;
 
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
}

```
</details>