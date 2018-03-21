# 第6章 函数式对象
> 

## 6.1 类Rational的规格说明书
有理数（rational number）

数学上有理数不具有可变的状态。有理数可以相加，但结果是新的有理数，原来的数不会被“改变”。我们本章设计的不可变的Rational类将秉承这一属性。每个有理数都能被表示成Rational对象。当两个Rational对象相加时，将创建新的而带着累加器结果的Rational对象。

## 6.2 创建Rational
```scala
class Rational(n:Int, d:Int)
```

* 如果类没有主体，就不需要指定一对空的花括号。
* 在类名Ration之后的括号里的n和d，被称为类参数。
* Scala编译器会收集这两个参数并创建出带同样参数的主构造器。

**注意**：这个最初的Rational例子凸显了Java和Scala之间的不同。Java类具有可以带参数的构造器，而Scala类可以直接带参数。Scala的写法更简洁——类参数可以直接在类的主体中使用；没必要定义字段然后写赋值函数把构造器的参数复制到字段里。这在无形中省略了很多固定写法，尤其是对小类来说。

Scala编译器将把类内部的任何既不是字段也不是方法定义的代码编译至主构造器中。如下：
```
class Rational(n: Int, d: Int) {
    println("Created " + n + "/" + d)
}
```

## 6.3 重新实现toString方法
* 默认情况下，Rational继承了`java.lang.Object`类的toString方法得到了这个貌似无稽的字符串。

```
class Rational(n: Int, d: Int) {
    override def toString = n + "/"+ d
}
```

## 6.4 检查先决条件

```
class Rational(n: Int, d: Int) {
    // require 方法定义在Predef对象中传入值为真，require正常返回。反之，require将抛出java.lang.IllegalArgumentException异常
    require(d!=0)
    override def toString = n + "/"+ d
}
```

## 6.5 添加字段
```
class Rational(n: Int, d: Int) {
    require(d!=0)
    val numer: Int = n
    val denom: Int = d
    override def toString = n + "/"+ d
    def add(that: Rational): Rational = {
        new Rational (
            numer * that.denom + that.numer * denom,
            denom * that.denom
        )
    }
}
```

## 6.6 自指向
```
class Rational(n: Int, d: Int) {
    require(d!=0)
    val numer: Int = n
    val denom: Int = d
    override def toString = n + "/"+ d
    def add(that: Rational): Rational = {
        new Rational (
            numer * that.denom + that.numer * denom,
            denom * that.denom
        )
    }
    def lessThan(that: Rational) = 
        this.numer * that.denom < that.numer * this.denom
    def max(that: Rational) = 
        if (this.lessThan(that)) that else this
}
```

## 6.7 辅助构造器
```
class Rational(n: Int, d: Int) {
    require(d!=0)
    val numer: Int = n
    val denom: Int = d
    // 辅助构造器
    def this(n: Int) = this(n, 1)
    override def toString = n + "/"+ d
    def add(that: Rational): Rational = {
        new Rational (
            numer * that.denom + that.numer * denom,
            denom * that.denom
        )
    }
    def lessThan(that: Rational) = 
        this.numer * that.denom < that.numer * this.denom
    def max(that: Rational) = 
        if (this.lessThan(that)) that else this
}
```

Scala中里的每个辅助构造器的第一个动作都是调用同类别的构造器。换句话说就是，每个Scala类的每个辅助构造器都是以“this\(\.\.\.\)”形式开头的。被调用的构造器既可以是主构造器，也可以是源文件中早于调用构造器定义的其他辅助构造器。规则的根本结果就是每个Scala的构造器调用终将结束于对主构造器的调用。因此主构造器是类的唯一入口点。

**注意**：Scala的类里面只有主构造器可以调用超类的构造器。

## 6.8 私有字段和方法
```
class Rational(n: Int, d: Int) {
    require(d!=0)
    private val g = gcd(n.abs, d.abs)
    val numer: Int = n
    val denom: Int = d
    // 辅助构造器
    def this(n: Int) = this(n, 1)
    override def toString = n + "/"+ d
    def add(that: Rational): Rational = {
        new Rational (
            numer * that.denom + that.numer * denom,
            denom * that.denom
        )
    }
    def lessThan(that: Rational) = 
        this.numer * that.denom < that.numer * this.denom
    def max(that: Rational) = 
        if (this.lessThan(that)) that else this
    private def gcd(a:Int, b: Int): Int = 
        if (b == 0) a else gcd(b, a % b)
}
```

## 6.9 定义操作符
```
class Rational(n: Int, d: Int) {
    require(d!=0)
    private val g = gcd(n.abs, d.abs)
    val numer: Int = n
    val denom: Int = d
    // 辅助构造器
    def this(n: Int) = this(n, 1)
    override def toString = n + "/"+ d
    def +(that: Rational): Rational = {
        new Rational (
            numer * that.denom + that.numer * denom,
            denom * that.denom
        )
    }
    def lessThan(that: Rational) = 
        this.numer * that.denom < that.numer * this.denom
    def max(that: Rational) = 
        if (this.lessThan(that)) that else this
    private def gcd(a:Int, b: Int): Int = 
        if (b == 0) a else gcd(b, a % b)
}
```

## 6.10 Scala的标识符
标识符构成方式：
* 字母数字标识符（alphanumeric identifier）以字母或下划线开始，之后可以跟字母、数字或下划线。 `$` 字符也被当作是字母，但被保留作为Scala编译器产生的标识符之用。用户程序里的标识符不应该包含`$` 字符，尽管能够编译通过；但这样做有可能会导致与Scala编译器产生的标识符发生名称冲撞。
* 操作符标识符（operator identifier）由一个或多个操作符字符组成。操作符字符是一些如\+， \:， \?， \~或\#的可打印的ASCII字符。Scala编译器将在内部“粉碎”操作符标识符以转换成合法的内嵌`$` 的标识符。例如，标识符`:->`将被内部表达为`$colon$minus$greater`。若你想从Java代码访问这个标识符，就应该使用这种内部表达方式。
* 混合标识符（mixed identifier）由字母数字组成，后面跟着下划线和一个操作符标识符。如unary\_\+被用做定义一元的“\+”操作符的方法名。或myvar\_\=被用做定义赋值操作符的方法名。（混合标识符myvar\_\=是由Scala编译器产生的用来支持属性的）
* 字面量标识符（literal identifier）是用反引号\`\.\.\.\`包括的任意字符串。你可以把运行时环境认可的任意字符串放在反引号之间当作标识符。结果总被当作是Scala标识符。即使包含在反引号间的名称是Scala保留字，这个规则也有效。在Java的Thread类中访问静态的yield方法是它的典型用例。你不能写`Thread.yield()`，因为yield是Scala的保留字。然而可以在反引号里引用方法的名称，例如```Thread.`yield`()```

## 6.11 方法重载
```
class Rational(n: Int, d: Int) {
    require(d!=0)
    private val g = gcd(n.abs, d.abs)
    val numer: Int = n
    val denom: Int = d
    // 辅助构造器
    def this(n: Int) = this(n, 1)
    override def toString = n + "/"+ d
    def +(i: Int): Rational = {
        new Rational (
            numer + i * denom,
            denom
        )
    }
    def +(that: Rational): Rational = {
        new Rational (
            numer * that.denom + that.numer * denom,
            denom * that.denom
        )
    }
    def lessThan(that: Rational) = 
        this.numer * that.denom < that.numer * this.denom
    def max(that: Rational) = 
        if (this.lessThan(that)) that else this
    private def gcd(a:Int, b: Int): Int = 
        if (b == 0) a else gcd(b, a % b)
}
```

## 6.12 隐式转换
如果r为Rational实例，执行`2 * r`，编译器会调用`2.*(r)`，Int中没有`Int.*(r: Rational)`方法，所以报错。
```
implicit def intToRationl(x: Int) = new Rational(x)
```
定义隐式转换后执行`2 * r`不报错。

## 6.13 一番告诫

## 6.14 小结