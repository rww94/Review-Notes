<!-- TOC -->

- [SSM框架](#ssm框架)
- [1 Spring常用注解](#1-spring常用注解)
- [2 JavaBean的声明周期](#2-javabean的声明周期)
- [3 项目打包过程](#3-项目打包过程)
- [4 Mybatis配置](#4-mybatis配置)
- [5 数据库连接池参数](#5-数据库连接池参数)

<!-- /TOC -->
# SSM框架
# 1 Spring常用注解
# 2 JavaBean的声明周期
# 3 项目打包过程
* IDEA -> File  -> Project Structure
* Project Settings -> Artifacts -> + -> Web Application: Exploded（在output目录中生成文件夹）
* Project Settings -> Artifacts -> + -> Web Application: Archive（选择上一步生成的文件夹名称，会在输入目录生成.war包）
* 将文件夹或war包拷贝到tomcat的wabapps目录下，然后在浏览器输入localhost:端口/文件名称
# 4 Mybatis配置

# 5 数据库连接池参数
* initialSize 初始化连接数目 
* maxActive 最大连接数
* maxWait 连接池中连接用完时，新的请求等待时间（毫秒）
* removeAbandonedTimeout 活动连接的最大空闲时间,单位为秒 超过此时间的连接会被释放到连接池中,针对未被close的活动连接  
