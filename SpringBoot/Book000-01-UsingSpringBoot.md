> 使用Spring Boot

# 使用Maven插件创建SpringBoot应用

## 创建SpringBoot应用

### 继承父Starter

在工程中添加如下配置：
``` xml
<!-- Inherit defaults from Spring Boot -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.0.RELEASE</version>
</parent>
```

> Note:这里需要指定Spring Boot依赖版本号，如果导入额外的组件，你可以安全的省略版本号。

在采用这种安装方式时如果你希望覆盖个别依赖，可在工程中添加property属性。例如，升级Spring Data，你可以添加如下内容到`pom.xml`：

``` xml
<properties>
<spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
</properties>
```
### 不在POM文件中使用parent标签

在某些情况下我们可能不希望使用parent标签，此时我们可以通过依赖管理中的`scope=import`来引入Spring。

``` xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <!-- Import dependency management from Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.0.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
如果采用这种方式，当我们想要覆盖个别依赖，不能采用设置property属性的方式，此时我们可以在依赖管理中声明`spring-boot`依赖之前声明需要覆盖的依赖，如下：
``` xml
<dependencyManagement>
    <dependencies>
        <!-- Override Spring Data release train provided by Spring Boot -->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-releasetrain</artifactId>
            <version>Fowler-SR2</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.0.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 使用Spring Boot Maven插件
Spring Boot包含一个Maven插件，它可以工程打包成一个可执行jar，插件使用方法如下：

```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin    </artifactId>
        </plugin>
    </plugins>
</build>
```

## Starters
Starters是一组方便的依赖描述，可以嵌入到你的应用中。你可以一站式构建Spring应用。

> 官方starters遵循相同的命名规则，`spring-boot-starter-*`，`*`代表特定类型的应用。这种命名结构旨在帮助你找到合适的starter。多种集成Maven的IDE环境支持根据名称搜索依赖，例如安装了STS插件的Eclipse，你可以在POM编辑器中按`ctrl-space`并输入“`spring-boot-starter`”显示提示列表。

# 构建你的代码
Spring Boot不需要特殊的代码才能正常工作。然而，下面的一些实践是很有帮助的。

## 使用“默认”包

当一个类中没有包声明时，被认为是在默认包中。不推荐使用默认包，应该尽量避免。它可能对一些使用`@ComponentScan`、`@EntityScan`或`@SpringBootApplication`注解的Spring Boot应用造成一些特殊问题，因为每个jar中的类都会被访问。

## 定位应用主类

推荐将应用主类放在其他类的顶层（根包）。`@EnableAutoConfiguration`常注解在主类上，这样就隐式定义一个基本搜索路径。例如，如果你编写一个JPA应用，会自动从`@EnableAutoConfiguration`注解的类所在的包中搜索包含`@Entity`的类。

使用顶层包时存放主类，且指定`@ComponentScan`注解时不需要特别指定其`basePackage`属性。也可以使用`@SpringBootApplication`注解。

典型的代码布局如下：

```
com
    +- example
        +- myapplication
        +- Application.java
        |
        +- customer
        | +- Customer.java
        | +- CustomerController.java
        | +- CustomerService.java
        | +- CustomerRepository.java
        |
        +- order
        +- Order.java
        +- OrderController.java
        +- OrderService.java
        +- OrderRepository.java
```

`Application.java`文件中声明`main`方法，且包含基本注解`@Configuration`。

``` java
package com.example.myapplication;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableAutoConfiguration
@ComponentScan
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

# 配置类

Spring  Boot支持基于Java的配置。尽管可以通过XML源来使用SpringApplication，一般推荐用单个`@Configuration`注解类作为主配置源。通常定义`main`方法的类是最适合作为主配置类。

> 互联网上有很多基于XML配置的Spring配置示例。如果可能的话，多尝试使用等价的基于Java的配置。搜索`Enable*`注解是一个很好的起点。

## 导入其他的配置类

你不需要将所有配置都放在一个注有`@Configuration`的配置类中。`@Import`注解可以导入其他的注解类。或者使用`@ComponentScan`注解自动查找Spring组件，包括注解`@Configuration`的类。

## 导入XML配置

如果你必须使用基于XML的配置，仍建议通过`@Configuration`类启动。然后通过`@ImportResource`注解加载XML配置文件。

# 自动配置

Spring Boot自主配置会根据Spring应用添加的jar依赖自动配置。例如，如果HSQLDB在你的classpath下，你不需要手动配置任何数据库连接bean，Spring Boot自主配置一个内存数据库。

你可以选择加入自主配置通过添加`@EnableAutoConfiguration`或`@SpringBootApplication`注解到一个`@Configuration`类上。

> 你应该只添加一个`@EnableAutoConfiguration`注解。一般推荐将他添加到主`@Configuration`类上。

## 逐步替换自主配置

自主配置是非侵入性的。任何时候，你可以自定义配置来替换部分自主配置。例如，如果你添加你自己的数据源bean，会取消默认的内嵌数据库。

添加`--debug`开关可以查看当前应用的自主配置内容，这样做会开启debug日志并在控制台输出。

## 禁用特定自主配置类

如果你发现某些不愿应用的自主配置类被应用，你可以使用` @EnableAutoConfiguration`的`exclude`属性排除，如下面的例子：

``` java
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jdbc.*;
import org.springframework.context.annotation.*;

@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```

如果类不在classpath下，你可以使用注解的`excludeName`属性来指定全限定名。最后，你也可以使用`spring.autoconfigure.exclude`属性来控制自主配置类列表。

> 你可以通过注解和属性来定义排除项。

# Spring Bean和DI

你可以自由使用任何一个标准的Spring框架技术去定义bean和依赖注入。例如，下面注解可以正常使用：`@ComponentScan`（查询bean）、`@Autowired`（构建注入）。
















































































































