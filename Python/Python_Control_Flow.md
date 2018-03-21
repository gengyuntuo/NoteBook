# Python 控制流程
>

## 分支：`if`
* Python中的所有对象都支持比较操作
* if语句格式：
```python
#!/usr/bin/python
# encoding=utf8
if True or False :
    pass
elif True or False :
    pass
else:
    pass

#  三目运算符，类似于Java中的 bool_express？express1：express2
val = 'Value One' if True or False else 'Value Two'
```

## 循环：`for`、`while`
* `while`循环的格式
```python
while True or False :
    pass
else:
    pass
```
如果使用`break`语句跳出循环体，else 后的代码不会被执行，即else代码块只在正常循环结束时才会执行
* `for`循环
```
for var in range(0, 101):
    pass
else:
    pass
 ```

## 列表解析
列表解析是Python迭代的一种应用，它常用于创建新的列表，因此要放在`[]`中

语法：
 * `[expression for iter_var in iterable]`
 * `[expression for iter_var in iterable if cond_expr]`

## 生成器
采用生成器表达式并不真正的创建数字列表，而是返回一个生成器对象，此对象每次计算出一个条目后，把这个条目产生\(yield\)出来
生成器的表达式使用了“惰性计算”或称作“延迟求值”的机制，因此序列过长，并且每次只需要获取一个元素时，应当考虑使用盛恒器表达式而不是列表解析
* 语法：
  * `(expr for iter_var in iterable)`
  * `(expr for iter_var in iterable if cond_expr)`

## enumerate
* range可在非完备遍历中生成索引偏移，而非偏移处的元素
* 如果同时需要偏移索引和偏移幸免元素，则可以使用enumurate\(\)函数
* 次内置函数返回一个生成器对象


