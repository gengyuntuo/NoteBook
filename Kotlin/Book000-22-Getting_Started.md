# Getting Started
> * [基本语法](#basic_syntax)
> * [方言](#idioms)
> * [代码约定](#coding_conventions)

## <a name="basic_syntax" />基本语法
### 包定义
包定义应该在源代码的顶部：
```kotlin
    package my.demo
    import java.util.*
    // ...
```
定义包名后不需要像Java一样将文件放置在和包名匹配的文件夹中，也就是说你可以随意放置这些文件
参见[包](#packages)

### 函数定义
* 定义一个有两个`Int`类型的参数和返回`Int`类型的函数，如下：
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```
* 带有表达式体和自动推断返回类型的函数：
```kotlin
fun sum(a: Int, b: Int) = a + b
```
* 返回值没有意义的函数定义：
```kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
```
* `Unit`返回值也可以这样写：
```kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
```
参考[函数](#functions)

### 定义局部变量
* 赋值一次\(只读\)局部变量
```
    val a: Int = 1 // 立即赋值
    val b = 2      // 自动推断类型为`Int` 
    val c: Int     // 指定类型，需要时初始化
    c = 3          // 延迟赋值
```
* 可变变量
```
    var x = 5 // `Int` type is inferred
    x += 1
```
参考[属性和字段](#properties_and_fields)

### 注释
* 像Java和JavaScript一样，Kotlin也支持行内注释和块注释
```kotlin
    // This is an end-of-line comment
    /* This is a block comment
    on multiple lines. */
```
和Java不同的是，在Kotlin中块注释是可以嵌套的
参考[Kotlin文档注释](#document_kotlin_code)

### 字符串模板
```
    var a = 1
    // simple name in template:
    val s1 = "a is $a"
    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
```
参见[字符串模板](#string_templates)

### 条件表达式
```kotlin
 fun maxOf(a: Int, b: Int): Int {
     if (a > b) {
        return a
     } else {
        return b
    }
 }
 ```
使用`if`的表达式：
```kotlin
    fun maxOf(a: Int, b: Int) = if (a > b) a else b
 ```
 参见[`if`表达式](#if_expressions)
 
 ### 空值和`null`检测
 一个可能为`null`的引用必需标记为允许为空。
 
 
 
 