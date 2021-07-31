# 装饰器
### 装饰器(decorator)功能

- 引入日志
- 函数执行时间统计
- 执行函数前预备处理
- 执行函数后清理功能
- 权限校验等场景
- 缓存

```python
# 定义函数：完成包裹数据
def makeBold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

# 定义函数：完成包裹数据
def makeItalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makeBold
def test1():
    return "hello world-1"

@makeItalic
def test2():
    return "hello world-2"

@makeBold
@makeItalic
def test3():
    return "hello world-3"

print(test1())
print(test2())
print(test3())
```
运行结果

```
<b>hello world-1</b>
<i>hello world-2</i>
<b><i>hello world-3</i></b>
```
### 1.无参数的函数

```python
rom time import ctime, sleep

def timefun(func):
    def wrapped_func():
        print("%s called at %s" % (func.__name__, ctime()))
        func()
    return wrapped_func

@timefun
def foo():
    print("I am foo")

foo()
sleep(2)
foo()
```
上面代码理解装饰器执行行为可理解成

```
foo = timefun(foo)
# foo先作为参数赋值给func后,foo接收指向timefun返回的wrapped_func
foo()
# 调用foo(),即等价调用wrapped_func()
# 内部函数wrapped_func被引用，所以外部函数的func变量(自由变量)并没有释放
# func里保存的是原foo函数对象
```


### 2.被装饰的函数有参数

```python
from time import ctime, sleep

def timefun(func):
    def wrapped_func(a, b):
        print("%s called at %s" % (func.__name__, ctime()))
        print(a, b)
        func(a, b)
    return wrapped_func

@timefun
def foo(a, b):
    print(a+b)

foo(3,5)
sleep(2)
foo(2,4)
```


### 3.被装饰的函数有不定长参数


```
from time import ctime, sleep

def timefun(func):
    def wrapped_func(*args, **kwargs):
        print("%s called at %s"%(func.__name__, ctime()))
        func(*args, **kwargs)
    return wrapped_func

@timefun
def foo(a, b, c):
    print(a+b+c)

foo(3,5,7)
sleep(2)
foo(2,4,9)
```

### 4.装饰器中的return

```
from time import ctime, sleep

def timefun(func):
    def wrapped_func():
        print("%s called at %s" % (func.__name__, ctime()))
        func()
    return wrapped_func

@timefun
def foo():
    print("I am foo")

@timefun
def getInfo():
    return '----hahah---'

foo()
sleep(2)
foo()


print(getInfo())
```
执行结果

```
foo called at Fri Nov  4 21:55:35 2016
I am foo
foo called at Fri Nov  4 21:55:37 2016
I am foo
getInfo called at Fri Nov  4 21:55:37 2016
None
```
如果修改装饰器为return func()，则运行结果：

```
foo called at Fri Nov  4 21:55:57 2016
I am foo
foo called at Fri Nov  4 21:55:59 2016
I am foo
getInfo called at Fri Nov  4 21:55:59 2016
----hahah---
```
总结：
一般情况下为了让装饰器更通用，可以有return

### 5.装饰器带参数,在原有装饰器的基础上，设置外部变量

```
#decorator2.py

from time import ctime, sleep

def timefun_arg(pre="hello"):
    def timefun(func):
        def wrapped_func():
            print("%s called at %s %s" % (func.__name__, ctime(), pre))
            return func()
        return wrapped_func
    return timefun

# 下面的装饰过程
# 1. 调用timefun_arg("itcast")
# 2. 将步骤1得到的返回值，即time_fun返回， 然后time_fun(foo)
# 3. 将time_fun(foo)的结果返回，即wrapped_func
# 4. 让foo = wrapped_fun，即foo现在指向wrapped_func
@timefun_arg("itcast")
def foo():
    print("I am foo")

@timefun_arg("python")
def too():
    print("I am too")

foo()
sleep(2)
foo()

too()
sleep(2)
too()
```
可以理解为：

```
foo()==timefun_arg("itcast")(foo)()
```

### 6.类装饰器

> 装饰器函数其实是这样一个接口约束，它必须接受一个callable对象作为参数，然后返回一个callable对象。在Python中一般callable对象都是函数，但也有例外。只要某个对象重写了 __call__() 方法，那么这个对象就是callable的。


```
class Test():
    def __call__(self):
        print('call me!')

t = Test()
t()  # call me
```
