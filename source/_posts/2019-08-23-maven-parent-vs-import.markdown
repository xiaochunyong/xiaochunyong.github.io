---
layout:     post
title:      "Maven 之  Parent vs Import Pom"
date:       2019-08-23T15:40:53+08:00
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - maven
---
> 我希望一个项目下, 多个模块下的依赖能统一管理. 因此开了个父pom. 由于在一个项目空间下, 每次添加依赖的时候, 都会想着先在父pom中添加, 再到子pom中添加. 变得繁琐了.
> 而且当需要将model模块部署到nexus仓库时, 还需将父pom也部署上去.  但是父pom的artifact名字语义又不对. 所以我想是否能把依赖拎出来形成一个公司部门级别的依赖通过import的方式来解决. 在使用import的方式导入pom 发现只能导入该pom中的dependencyManagement中的定义, 不能导入pluginManagement中的定义.  所以又想是否可以再定义一层父pom. 命名为department-parent继承自spring-boot-starter-parent, department-parent包含对公司其它部门项目的依赖及第三方依赖, 然后让部门其它项目都继承自department-parent. 和同事讨论后得出结论. 如果以这种方式进行, 涉及到版本更新, 没人愿意去升级. 如果是稳定的没有动力升级,  如果依赖变更大, 直接升级父pom. 会使得所有未指定版本号的都为父pom中的版本号. 风险太大. 同事更多会选择自己写版本号自己控制. (找到好的画图软件后画个图吧)



<!--V1.
parent为org.springframework.boot:spring-boot-starter-parent
creative-itm-package
- itm-app
- itm-model-->





## Import Pom

```xml
<dependencyManagement>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>${spring.boot.version}</version>
        <type>pom</type>
        <scope>import</scope>
    </dependency>
<dependencyManagement>
```
> 优点: 可以在dependencyManagement节点下引入多个import pom依赖. 提供使用.
> 缺点: 引入的依赖只有里面的dependencyManagement会被合并到当前pom.xml文件


## Parent

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.6.RELEASE</version>
</parent>
```
> 优点: 父pom所有属性都将被当前pom继承(可以覆盖), 包括dependencyManagement和pluginManagement等.
> 缺点: 只能单继承. 



当然, 二者可以结合使用.