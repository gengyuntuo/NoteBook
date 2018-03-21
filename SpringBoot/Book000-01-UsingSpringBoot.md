> 使用Spring Boot

# 使用Maven插件创建SpringBoot应用

## 创建方式

### 继承父Starter

在工程中添加如下配置：
```
<!-- Inherit defaults from Spring Boot -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.0.RELEASE</version>
</parent>
```

> Note:这里需要指定Spring Boot依赖版本号，如果导入额外的组件，你可以安全的省略版本号。

在采用这种安装方式时如果你希望覆盖个别依赖，可在工程中添加一个属性。例如，升级Spring Data，你可以添加如下内容到`pom.xml`：

```
<properties>
<spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
</properties>
```


