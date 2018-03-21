# Python 面向对象编程
>

## 类间关系
* 依赖：一个类的方法操作另外一个类的对象
* 聚合：类A的对象包含类B的对象
* 继承：描述特殊与一般关系

## 面向对象特点：封装、继承及多态
* 封装
* 继承
* 多态

## Python类和实例
* 类时一种数据结构，可以用来创建实例
* Python类是一个可调用对象，即类对象
* Python2\.2之后，类是一种自定义类型，而实例则是声明某个自定义类型的变量
* 实例初始化
  * 通过调用类来创建实例：`instance = ClassName9(args...)`
  * 类在实例化时可以使用`__init__`和`__del__`两个特殊的方法

## Python中创建类
* Python使用class关键字创建类，语法格式如下
```python
class BaseClass :
    def __init__(self): 
        pass
    pass
class ClassName(BaseClass):
    """Class documentation string"""
    pass
```
* 超类是一个或多个用于继承的父类的集合
* 类体可以包含：声明语句、类成员定义、数据属性、方法

**注意**：
 * 如果不存在继承关系，ClassName后面的`(BaseClass)`可以不提供
 * 类文档为可选
