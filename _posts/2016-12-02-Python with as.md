---
layout: post
title: Python with...as..的使用
categories: 数据分析
tags: Python
---

# Python的with...as的用法  

这个语法是用来代替传统的try...finally语法的。 

```
with EXPRESSION [ as VARIABLE] WITH-BLOCK 
```

基本思想是with所求值的对象必须有一个__enter__()方法，一个__exit__()方法。  

紧跟with后面的语句被求值后，返回对象的__enter__()方法被调用，这个方法的返回值将被赋值给as后面的变量。当with后面的代码块全部被执行完之后，将调用前面返回对象的__exit__()方法。  

```python
file = open("/tmp/foo.txt")
try:
    data = file.read()
finally:
    file.close()
```

使用with...as...的方式替换，修改后的代码是：

```python
with open("/tmp/foo.txt") as file:
    data = file.read()
```

```python
#!/usr/bin/env python
# with_example01.py
 
 
class Sample:
    def __enter__(self):
        print "In __enter__()"
        return "Foo"
 
    def __exit__(self, type, value, trace):
        print "In __exit__()"
 
 
def get_sample():
    return Sample()
 
 
with get_sample() as sample:
    print "sample:", sample
```

执行结果为

```
In __enter__()
sample: Foo
In __exit__()
```

1. \__enter\__()方法被执行

2. \__enter\__()方法返回的值 - 这个例子中是"Foo"，赋值给变量'sample'

3. 执行代码块，打印变量"sample"的值为 "Foo"

4. \__exit\__()方法被调用with真正强大之处是它可以处理异常。

