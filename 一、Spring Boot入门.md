## 一、Spring Boot入门

#### 1、Spring Boot简介

Spring Boot来简化Spring应用开发，约定大于配置，去翻从简，just run就能创建一个独立的，产品级别的应用。整个Spring技术栈的一个大整合。

**背景：**

J2EE笨重的开发，繁多的配置，低下的开发效率，复杂的部署流程，第三方技术集成难度大

**解决：**

“Spring全家桶”时代

Spring Boot--->J2EE一站式解决方案

Spring -->分布式整体解决方案

**优点：**

- 快速创建独立运行的Spring项目以及与主流框架集成
- 使用嵌入式的Servlet容器，应用无需打成war包（Springboot把应用打成一个jar包，用java -jar的方式来运行）
- starters自动依赖与版本控制（每一部分都有相应的starters启动起，比如我们想用web功能就导入web的starters，想用JDBC相关功能就导入jdbc相关的starters）
- 大量的自动配置，简化开发，也可修改默认值
- 无需配置XML，无代码生成，开箱即用（告别大量的xml编写，应用springboot里面写好的api，比如springboot.porperties或者springboot.yml）
- 准生产环境的运行时应用监控
- 与云计算的天然集成

#### 2、微服务

2014年， martinfowler

[Microservices]: https://www.martinfowler.com/articles/microservices.html

微服务：架构风格

一个应用应该是一组小型服务；可以通过HTTP的方式进行互通。

**单体应用**ALL IN ONE

![单体应用](C:\Users\Administrator\Desktop\SpringBoot文件\images\传统项目架构.png)

**微服务**

![微服务](C:\Users\Administrator\Desktop\SpringBoot文件\images\微服务.jpg)

**单体应用：**一个单体应用程序把它所有的功能放在一个单一进程中，并且通过在多个服务器上复制这个单体进行扩展。

**微服务：**一个微服务架构把每个功能元素放进一个独立的服务中，并且通过跨服务器分发这些服务进行扩展，只在需要时才复制。

每一个功能元素最终都是一个可独立替换和独立升级的软件单元。

**环境约束：**

- JDK1.8 :Spring Boot要JDK1.7以及以上
- Maven3.x:maven3.3以上版本
- IntelliJ IDEA 2018.2.4 x64
- SpringBoot 1.5.9.RELEASE

![环境约束](C:\Users\Administrator\Desktop\SpringBoot文件\images\开发环境.png)

### 1、Maven设置：

给maven的settings.xml配置文件的profiles标签添加

```xml
<profile>
      <id>jdk-1.8</id>
      <activation>
		<activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
	  <properties>
		<maven.comiler.source>1.8</maven.comiler.source>
		<maven.comiler.target>1.8</maven.comiler.target>
		<maven.comiler.compilerVersion>1.8</maven.comiler.compilerVersion>
	  </properties>
    </profile>
    
```

### 2、IDEA设置

在idea的File-->Settings-->Build,Execution,Deployment-->Build Tools-->Maven设置如下：

![IDEA的Maven设置](C:\Users\Administrator\Desktop\SpringBoot文件\images\IDEA设置.png)

### 3、Spring Boot HelloWorld

一个功能：

浏览器发送hello请求，服务器接受请求并处理，响应Hello World字符串

#### 1、创建一个maven工程；（jar）

#### 2.导入依赖SpringBoot相关的依赖

```xml
	<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

#### 3、编写一个主程序

作用：启动SpringBoot应用

```java
package com.victor;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
/**
*@SpringBootApplication来标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        //Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

#### 4、编写相关的controller,service

```java
package com.victor.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * 标准这是一个controller
 */
@Controller
public class HelloController {
    /**
     * @ResponseBody表示把helloworld写给浏览器
     */
    @ResponseBody
    /**
     * @RequestMapping表示处理hello请求
     */
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World!";
    }
}
```

#### 5、运行主程序测试

![springboot测试](C:\Users\Administrator\Desktop\SpringBoot文件\images\springboot测试.png)

![springboot测试1](C:\Users\Administrator\Desktop\SpringBoot文件\images\springboot测试1.png)

#### 6、简化部署

```xml
    <!-- 这个插件，可以将应用打包成一个可执行的jar包-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

将这个项目打成jar包，直接使用java -jar的命令运行部署

![打成jar包](C:\Users\Administrator\Desktop\SpringBoot文件\images\打成jar包.png)

### 4、Hello World探究

#### 1、POM文件

##### 1、父项目

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
他的父项目是
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-dependencies</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath>../../spring-boot-dependencies</relativePath>
	</parent>
他来真正管理Spring Boot应用里面的所有依赖版本；
```

![springboot的依赖版本](C:\Users\Administrator\Desktop\SpringBoot文件\images\dependencies.png)

Spring Boot的版本仲裁中心；

以后我们导入依赖默认是不需要写版本（没有在dependencies里面管理的依赖自然需要声明版本号）

##### 2、导入的依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

**spring-boot-starter**-web:

​	spring-boot-starter:springboot场景启动器;帮我们导入了web模块正常运行所依赖的组件；

```xml
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
		</dependency>
	</dependencies>
```

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters启动器，只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景的启动器。

#### 2、主程序类，主入口类

```java
package com.victor;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
/*
@SpringBootApplication来标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        //Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}

```

**@SpringBootApplication**:Spring Boot应用标准在某个类上说明这个类时SpringBoot的主配置类，Spring Boot就应该运行这个类的main方法来启动SpringBoot启用

![@SpringBootApplication](C:\Users\Administrator\Desktop\SpringBoot文件\images\springboot注解.png)

**@SpringBootConfiguration**:Spring Boot的配置类；

​	标注在某个类上，表示这是一个Spring Boot的配置类

​	**@Configuration**:配置类上来标注这个注解；

​		配置类-----配置文件（利用配置类，来代替配置文件）；配置类也是容器中的一个组件@Component

![@SpringBootConfiguration](C:\Users\Administrator\Desktop\SpringBoot文件\images\configuration注解.png)

![@Component](C:\Users\Administrator\Desktop\SpringBoot文件\images\component注解.png)

**@EnableAutoConfiguration**:开启自动配置功能；

​	以前我们需要配置的东西，Spring Boot帮我们自动配置；**@EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置功能才能生效

![@EnableAutoConfiguration](C:\Users\Administrator\Desktop\SpringBoot文件\images\EnableAutoConfiguration注解.png)

​	**@AutoConfigurationPackage**:自动配置包

​		**@Import**（AutoConfigurationPackages.Registrar.class）:Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class	

​		**将主配置类（@SpringBootApplication标准的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；**

![@AutoConfigurationPackage](C:\Users\Administrator\Desktop\SpringBoot文件\images\AutoConfigurationPackage注解.png)

​	@**Import**（EnableAutoConfigurationImportSelector.class）;

​		给容器中倒入组件

​		**EnableAutoConfigurationImportSelector:**导入哪些组件的选择器；

![isEnable](C:\Users\Administrator\Desktop\SpringBoot文件\images\isEnable.png)

​		将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

![导包选择器](C:\Users\Administrator\Desktop\SpringBoot文件\images\Loader.png)

​		会给容器中倒入非常多的自动配置类(xxxAutoConfiguration);就是给容器中导入这个场景需要的所有组件，并配置好这些组件；

![debug1](C:\Users\Administrator\Desktop\SpringBoot文件\images\debug1.png)

![](C:\Users\Administrator\Desktop\SpringBoot文件\images\debug2.png)

​		有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；

```java
SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader);
```

SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值。将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作。以前我们需要自己配置的东西，自动配置类都帮我们做了。

J2EE的整体整合解决方案和自动配置都在

[jar包]: D:\SpringBoot开发IDE\apache-maven-3.5.4-bin\repository\org\springframework\boot\spring-boot-autoconfigure\1.5.9.RELEASE

### 5、使用Spring Initializer快速创建Spring Boot项目

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目

选择我们需要的模块；向导会联网创建Spring Boot项目；

默认生成的Spring Boot项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源；js css images;
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker，thymeleaf）；
  - application.properties:Spring Boot应用的配置文件；可以修改一些默认设置；

## 二、配置文件

### 1、配置文件

SpringBoot使用一个全局的配置文件，配置文件名师固定的；

- application.properties
- application.yml

配置文件的作用：修改SpringBoot自动配置的默认值；

配置文件放在**src/main/resources**目录或者类路径**/config**下

SpringBoot在底层都给我们自动配置好；

yml是**YAML**（YAML Ain't Markup Language）语言的文件，以**数据**为中心，比json、xml等更适合做配置文件

- [yaml]: http://www.yaml.org/	"参考语法规范"

  YAML  A Markup Language：是一个标记语言;

  YAML isn't Markup Language：不是一个标记语言；

  标记语言：

  ​	以前的配置文件；大多都是用的是**xxx.xml**文件；

  ​	YAML:**以数据为中心**，比json、xml等更适合做配置文件；

  ​	YAML：配置文件

  ```yaml
  server:
   port: 8081
  ```

  ​	XML:

  ```xml
  <server>
      <port>8081</port>
  </server>
  ```

### 2、YAML语言：

#### 1、基本语法

key:(空格)value: 表示一对键值对（空格必须有）；

以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
	port: 8081
	path: /hello
```

属性和值也是大小写敏感；

#### 2、值的写法

##### 1、字面量：普通的值（数字，字符串，布尔）

​	k: v: 字面直接来写；

​		字符串默认不用加上单引号或者双引号

​		"":双引号会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​			name: "zhangsan \n lisi";输出：zhangsan 换行 lisi

​		'':单引号；不会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​			name:'zhangsna \n lisi';输出：zhangsan \n lisi

##### 2、对象、Map（对象和值）（键值对）：

​	k: v:  在下一行来写对象的属性和值的关系；注意缩进

​		对象还是k:v的方式

```yaml
friends:
	lastName: zhangsan
	age: 20
```

行内写法：

```yml
friends: {latName: zhangsan,age: 20}
```

##### 3、数组（List，Set）：

用- 值表示数组中的一个元素(ps:-后面有空格)

```yaml
pets:
	- cat
	- dog
	- pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```

#### 3、获取配置文件值（配置文件值注入）

配置文件

```yaml
person:                
  lastName: zhangsan   
  age: 18              
  boss: false          
  birth: 2019/01/07    
  maps: {k1: v1,k2: v2}
  lists:               
    - lisi             
    - zhaoliu          
  dog:                 
    name: 小狗           
    age: 12            
```

对应的JavaBean：

```java
package com.example.demo.com.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.*;
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfirgurationProperties:告诉SpringBoot将本类中的所有的属性和配置文件中相关的配置进行绑定
 * prefix="person"：配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件是容器中的组件，才能用容器提供的@ConfigurationProperties功能,所以必须使用@Component注解
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

    @Override
    public String toString() {
        return "Person{" +
                "lastName='" + lastName + '\'' +
                ", age=" + age +
                ", boss=" + boss +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getBoss() {
        return boss;
    }

    public void setBoss(Boolean boss) {
        this.boss = boss;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }
}

```

我们可以导入配置文件处理器，以后编写配置就有提示了

```xml
        <!--导入配置文件处理器，配置文件进行绑定就会有提示-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

##### 1、@Value获取值和@ConfigurationProperties获取值比较

|                      | @ConfigurationProperties | @Value       |
| -------------------- | ------------------------ | ------------ |
| 功能                 | 批量注入配置文件中的属性 | 一个个值指定 |
| 松散绑定（松散语法） | 支持松散绑定             | 不支持       |
| SpEL                 | 不支持                   | 支持         |
| JSR303数据校验       | 支持                     | 不支持       |
| 复杂类型封装         | 支持                     | 不支持       |

配置文件yml还是properties他们都能获取到值；

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用**@Value**

如果说，我们专门编写了一个JavaBean来和配置文件进行映射，我们就直接使用**@ConfigurationProperties**

（松散绑定）属性名匹配规则（Relexed binding）

- person.lastName：使用标准方式
- person.last-name：大写用-
- person.last_name：大写用_
- PERSON_LAST_NAME:推荐系统属性使用这种写法

配置文件yml还是properties他们都能获取到值；

##### 2、配置文件注入值数据校验

``` java
@Component
@ConfigurationProperties(prefix = "person")
@Validated//启用数据校验
public class Person {
    /**
     * <bean class="Person">
     *     <property name="lastName" value="字面量/${key}从环境变量，配置文件中获取值/#{SpEL}"></property>
     * </bean>
     */
    //@Value("${person.last-name}")
    @Email//校验是否为电子邮件地址格式
    private String lastName;
```

##### 3、@PropertySource与@ImportResource

**@PropertySource:**加载指定的配置文件

``` java
package com.example.demo.com.bean;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;
import org.springframework.validation.annotation.Validated;

import javax.validation.constraints.Email;
import java.util.Date;
import java.util.*;
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfirgurationProperties:告诉SpringBoot将本类中的所有的属性和配置文件中相关的配置进行绑定
 * prefix="person"：配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件是容器中的组件，才能用容器提供的@ConfigurationProperties功能
 * @ConfigurationProperties(prefix = "person")默认从全局配置文件中获取值
 */
@PropertySource(value={"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
//@Validated
public class Person {
    /**
     * <bean class="Person">
     *     <property name="lastName" value="字面量/${key}从环境变量，配置文件中获取值/#{SpEL}"></property>
     * </bean>
     */
    //@Value("${person.last-name}")
    //@Email
    private String lastName;
    //@Value("#{11*2}")
    //@Value("${person.age}")
    private Integer age;
    //@Value("true")
    private Boolean boss;
    private Date birth;
   // @Value("${person.maps}")
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

    @Override
    public String toString() {
        return "Person{" +
                "lastName='" + lastName + '\'' +
                ", age=" + age +
                ", boss=" + boss +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getBoss() {
        return boss;
    }

    public void setBoss(Boolean boss) {
        this.boss = boss;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }
}

```

**@ImportResource:**导入Spring的配置文件，让配置文件里面的内容生效；

SpringBoot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别；

想让Spring的配置文件生效，加载进来；**@ImportResource**标准在配置类上



```java
package com.example.demo;
/*主配置类*/
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;
//导入spring的配置文件让其生效
@ImportResource(locations = {"classpath:beans.xml"})
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="helloService" class="com.example.demo.service.HelloService"></bean>
</beans>
```

SpringBoot推荐给容器中添加组件的方式；推荐使用全注解的方式

1、配置类======Spring配置文件

2、使用@Bean给容器中添加组件

```java
package com.example.demo.config;

import com.example.demo.service.HelloService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Configuration：指明当前类是一个配置类，就是来替代之前的Spring配置文件
 * 在配置文件中用<bean></bean>标签加组件
 */
@Configuration
public class MyAppConfig {
    //将方法的返回值添加到容器中，容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService(){
        System.out.println("配置类@Bean给容器中添加组件了");
        return new HelloService();
    }
}
```

#### 4、配置文件占位符

##### 1、随机数

```java
${random.value}、${random.int}、${random.long}
${random.int(10)}、${random.int[1024,65536]}
```

```properties
person.last-name=张三${random.uuid}
person.age=${random.int}
person.birth=2019/01/07
person.boss=false
person.maps.k1=v1
person.maps.k2=v2
person.lists=a,b,c
person.dog.name=${person.hello:hello}_dog
person.dog.age=15
```

##### 2、占位符获取之前配置的值，如果没有可以使用：指定默认值

```properties
#获取随机数
person.last-name=张三${random.uuid}
#person.hello没有这个属性，指定默认值为hello
person.dog.name=${person.hello:hello}_dog
```

#### 5、Profile

##### 1、多Profile文件

我们在主配置文件便携的时候，文件名可以是application-{profile}.properties或者application-{profile}.yml

默认使用application.properties的配置；

##### 2、yml支持多文档块方式

```yaml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8082
spring:
  profiles: dev
---
server:
  port: 8083
spring:
  profiles: prod  #指定属于哪个环境
```



##### 3、激活指定profile

​	1、在配置文件中指定spring.profiles.active=dev

​	2、命令行：

​		java -jar demo-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev

​		可以直接在测试的时候，配置传入命令行参数

​	3、虚拟机参数：

​		-Dspring.profiles.active=dev



#### 6、配置文件加载位置

springboot启动会扫描以下位置的application.properties或者application.yml文件作为SpringBoot的默认配置文件

-file:/config/（项目路径直接新建一个包config下的配置文件）

-file:/（项目路径）

-classpath:/config/（类路径下的新建一个包config下的配置文件）

-classpath:/（类路径下的配置文件）

优先级由高到低，高优先级的配置会覆盖低优先级的

SpringBoot会从这四个位置全部加载主配置文件；**互补配置**



