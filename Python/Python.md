# 虚拟环境

## virtualenv

```bash
pip install virtualenv

virtualenv --python=3.6 py36
source venv/bin/activate
```



# 类型

## 可迭代对象、生成器和迭代器

可迭代对象比较容易理解，比如 list、set 对象。

迭代器是一种支持 next() 的对象，它包含了一组元素，当执行 next() 操作时，返回其中一个元素。当所有元素都被返回之后再执行 next() 的时候就会报错。

对可迭代对象应用 iter 函数，可以得到一个迭代器。举例，

```python
l = [1, 2, 3]
a = iter(l)
print(a)  # list_iterator ...
print(next(a))  # 1
print(next(a))  # 2
print(next(a))  # 3
print(next(a))  # StopIteration Exception
```

生成器是一种特殊的迭代器。生成器本质上就是一个带 yiled 的函数，该记住了上一次返回时在函数中的位置。 举例，

```python
def createGenerator():
    mylist = range(3)   # range():返回的是一个可迭代对象
    for i in mylist:
        yield i
        
g = createGenerator()
print(g)  # generator ...
print(next(g))  # 1
print(next(g))  # 2
print(next(g))  # 3
```





# 内存管理

> [IT诸葛亮](https://www.jianshu.com/p/0914a5e9ae37)

一、对象的引用计数机制

Python内部使用引用计数来追踪内存中的对象，所有对象都有引用计数。

引用计数增加的情况：
1. 一个对象分配一个新名称
2. 将其放入一个容器中（如列表、元组或字典）

引用计数减少的情况：
1. 使用del语句对对象别名显示的销毁
2. 引用超出作用域或被重新赋值

sys.getrefcount()函数可以获得对象的当前引用计数。多数情况下，引用计数比你猜测得要大得多。对于不可变数据（如数字和字符串），解释器会在程序的不同部分共享内存，以便节约内存。

二、 垃圾回收

1. 当一个对象的引用计数归零时，它将被垃圾收集机制处理掉。
2. 当两个对象a和b相互引用时，del语句可以减少a和b的引用计数，并销毁用于引用底层对象的名称。然而由于每个对象都包含一个对其他对象的应用，因此引用计数不会归零，对象也不会销毁（从而导致内存泄露）。为解决这一问题，解释器会定期执行一个循环检测器，搜索不可访问对象的循环并删除它们。

三、内存池机制

Python提供了对内存的垃圾收集机制，但是它将释放的内存放到内存池而不是返回给操作系统。Python内存池用于用于管理对小块内存的申请和释放。

1. pymalloc机制。Python中所有小于256个字节的对象都使用pymalloc分配内存，而大的对象则使用系统的malloc。
3. 不同类型的内存池相互独立，不会共享相同的空间。如果分配又释放了大量的整数，用于缓存这些整数的内存空间就不能再分配给浮点数。

# 面向对象 
## __new__方法和__init__方法

1. new是类方法，init是实例方法。新建一个对象会自动调用new方法和init方法。new方法的调用是发生在init之前的。
2. new方法用来创建一个实例对象，并作为返回值返回，然后这个实例对象会调用init方法进行初始化。

## 静态方法和类方法

在Python中，静态方法和类方法都是类中的特殊方法，它们在内存中都归属于类，但它们的调用方式和自动传入的参数不同。

静态方法是一种不需要实例对象即可调用的方法，它既不依赖实例对象也不依赖于类，只是需要一个载体即可。无论是通过类对象直接调用还是实例对象进行调用都是可以的，需要注意的是在静态方法中无法使用实例属性和方法。

类方法是一种由类调用的方法，至少需要一个 cls 参数，执行类方法时，自动将调用该方法的类赋值给cls。类方法同样可以使用类名或实例名来调用，但通常建议使用类名。

总的来说，静态方法和类方法的使用场景不同，静态方法适合封装一些独立的功能，而类方法适合封装一些与类相关的功能，如操作类属性等



# 进程、线程和协程
> [Python的线程、进程和协程](https://www.cnblogs.com/delav/p/10927383.html)

1. 进程。一个进程就是一个正在运行的程序，它是CPU分配资源的基本单位。每个进程都有自己独立的内存空间。对于单核CPU，宏观上有多个进程同时在运行，但是微观上是CPU快速切换的结果。
2. 线程。线程是CPU调度的基本单元。线程存在于进程中。一个进程中至少有一个线程，多个线程能够共享进程的大部分资源。
3. 协程。协程是一种用户态的轻量级线程，又称微线程。协程拥有自己的寄存器上下文，调度切换时，将上下文状态进行保存。下一次进入时，能够回到上一次调用的状态。线程由CPU进行调度，协程由用户进行调度。
4. Python的多进程可由multiprocessing库实现；多线程可由threading库实现；协程可由gevent、asyncio或yield实现。

# GIL
GIL (global interpreter lock)，全局解释器锁。每个线程在执行前都需要先获取GIL，保证同一时刻只有一个线程能使用CPU，即同一时刻只有一个线程可以执行代码，所以Python中的多线程并不是真正意义上的多线程。不过GIL并不是Python的特性，它是CPython解析器中的一个概念，JPython解释器就没有这个概念。

# 反射
反射是一种能在运行时获得对象的变量或方法的机制。Python中的反射可以通过字符串对对象的变量或方法进行查找、增加、删除等操作。它有四个重要的方法：

- getattr: 获取指定字符串名称的属性
- setattr: 为对象设置一个属性
- hasattr: 判断对象是否有指定字符串名称的属性
- delattr: 删除指定字符串名称的属性


# 闭包
> [理解Python闭包概念](https://www.cnblogs.com/yssjun/p/9887239.html)

闭包并不只是一个Python中的概念，在函数式编程语言中应用比较广泛。闭包是一个引用了自由变量的函数。闭包可以让函数和变量脱离原本的作用域存在，使其长期存在于内存中。

Python闭包实现：在外层函数内定义一个内层函数，内层函数引用了外层函数的局部变量，并且外层函数的返回值是内层函数的引用。该局部变量称为自由变量，因为它在内层函数外定义，在内层函数中使用。

```python
def print_logs():
    logs = []
    def printer():
        logs.append("call printer()")
        print(logs)
    return printer  # 返回一个闭包
    
closure = print_logs()
closure()
closure()
```

闭包中引用的自由变量：
- 一个闭包实例对其自由变量的修改会被传递到下一次该闭包实例的调用。
- 闭包中引用的自由变量只和具体的闭包实例有关联，每个闭包实例引用的自由变量互不干扰。


# 装饰器
> [理解Python装饰器(Decorator)](https://www.jianshu.com/p/ee82b941772a)

Python装饰器是一个函数，可以让其他函数在不需要做任何代码处理的前提下增加额外的功能。比如：插入日志、性能测试等。

1. 不带参数的装饰器
```python
import functools
def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('call {}():'.format(func.__name__))
        return func(*args, **kwargs)
    return wrapper

@log
def test():
    print("hello world")
test()
    
# 等价于上面
def test():
    print("hello world")
test = log(test)
test()
```

2. 带参数的装饰器
```python
import functools

def log_with_param(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print('call {}():'.format(func.__name__))
            print('log_param = {}'.format(text))
            return func(*args, **kwargs)
        return wrapper
    return decorator
    
@log_with_param("param")
def test():
    print("hello world")
test()
    
# 等价于上面
def test():
    print("hello world")
decorator = log_with_param("param")
test = decorator(test)
test()
```

# 各种问题
## 变量查找规则
> [Python变量查找LEGB原则](https://blog.csdn.net/baidu_35085676/article/details/79851251)

LEGB规则：
- L-Local(function)：函数内的名字空间
- E-Enclosing function locals：外部嵌套函数的名字空间(例如closure)
- G-Global(module)：函数定义所在模块（文件）的名字空间
- B-Builtin(Python)：Python内置模块的名字空间

当Python访问变量值时，默认LEGB查找原则，如果都找不到，则会抛出NameError。