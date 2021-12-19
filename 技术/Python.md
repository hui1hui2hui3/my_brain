---
tag: ["Python"]
---

## 高阶函数
- **map**：对所有可迭代对象的每个元素，执行函数
> 生成器 = map(函数，可迭代对象)
> 函数可以使用lambda表达式

```
def mypow(x):
	return x**2
map(mypow,[1,2,3])
map(lambda x:x**2,[1,2,3])
map(lambda x,y:x+y,[1,2,3],[4,5,6]) #分别想加
```
- **reduce**: 对可迭代对象相邻的2个元素，执行函数
> 结果值 = reduce(函数，可迭代对象)
```
def add(x,y):
	return x+y
reduce(add,[1,2,3])
reduce(lambda x,y:x+y, [1,2,3]) #6 总和想加
```
- **filter**: 对可迭代对象的每个元素，执行函数确定过滤
> 生成器 = filter(函数，可迭代对象)
```
def odd(x):
	return x%2==0
filter(odd,[1,2,3])
filter(lambda x:x%2==0,[1,2,3])
```

## 推导式
**核心语句**：[映射表达式 for value in iterable if 过滤表达式]
正例：简单语句，一个for，方便理解阅读
```
[x for x in list if x%2=0]
[fun(x) for x in gen_iter() if filter_test(x)]
```

反例：多个for语句，难理解
```
[x,y for x in xlist if x/2=0 for y in ylist if y%2=0]
```

## 生成器
- yield
- yield from

## [[异步IO]]
## [[Python反射]]
## 装饰器
### 方法装饰器
- 普通方法装饰器
```
def decorator(fn):
	def wrapper(*args,**kwargs):
		return fn(*args,**kwargs)
	return wrapper

@decorator
def myfn()
	pass
```
相当于 `myfn = decorator(myfn)`
- 带参数的方法装饰器
```
def param_decorator(param):
	def decorator(fn):
		def wrapper(*args,**kwargs):
			return fn(*args,**kwargs)
		return wrapper
	return decorator

@decorator("choice")
def myfn()
	pass
	
```
相当于`myfn = decorator("choice")(myfn)`
### 类装饰器
需要重写[[#^764744|__call__]]函数
- 普通类装饰器
```
class Decorator():
	def __init__(self,fn):
		self.fn = fn
	def __call__(self,*args,**kwargs):
		return self.fn(*args,**kwargs)

@Decorator
def myfn():
	pass
```
相当于 `myfn = Decorator(myfn)`
- 带参数的类装饰器
```
class Decorator():
	def __init__(self,param):
		self.param = param
	
	def __call__(self,fn):
		def wrapper(*args,**kwargs):
			return fn(*args,**kwargs)
		return wrapper

@Decorator("haha")
def myfn():
	pass
```
相当于`myfn = Decorator("haha")(myfn)`

## [[Python测试框架]]
## 内置方法
- open 读取文件
> 返回一个拥有上下文管理器的 对象,简称file-like object；特点是实现了`__enter__`和`__exit__`方法
> 拥有上下文管理器的对象，可以使用with语句进入和退出上下文 
^15ed4f

```
def MyOpen():
	def __init__(self,path,mode)
		self.path = path
		self.mode = mode
	def __enter__(self):
		print("进入上下文")
		self.file = open(self.path,self.mode)
		return self.file
	
	def __exit__(self):
		self.file.close()
		print("退出上下文")

with MyOpen(path,'r') as f:
	print("进入代码块")
	print("end代码块")
```

^a6117d

> 打印结果是：
	> 进入上下文
	> 进入代码块
	> end代码块
	> 退出上下文

## 对象内置方法剖析
- `__new__`:方法，创建类实例，最优先执行；是静态方法
**原理**：
> python创建对象的底层方法，比init优先执行;
> 如果重写__new__后，且返回值不是当前类或器子类，则不再执行init方法

**注意1**：new的参数为cls而不是self
**注意2**：new如果返回为子类是，则真实对象为其子类，方法也是调用子类中的方法
```
class Father():  
    pass
class Test(Father):  
    def __init__(self):  
        print("init")  
  
    def __new__(cls):  
        print("new")  
        return object.__new__(Other)  #会执行init
		#return object.__new__(Test)  #会执行init
		#return object.__new__(Father )   #不会执行init
		#return None   #不会执行init
		#没有返回值      #不会执行init
  
class Other(Test):  
    pass  
  
t = Test()
```
**什么时候重写new？**
> 当为了防止一个类被init初始化时
- `__init__`：方法，创建类实例，new之后执行
对象初始化，比new晚执行，当new执行完毕时，如果返回的当前对象或子类，则会调用执行，否则不执行
- `__call__` ：方法,表明对象是否为callable对象

> 重写该方法可以是对象变为一个Callable对象，可以像函数一样执行调用
> 所有callable都一定有__call__，所以可以根据是否有__call__判断，对象内的属性是否为函数
```
class CallObj():
	def __call__(self,name):
		print(name)
		
call_obj = CallObj()
call_obj("haha")  #可以像函数一样调用
call_obj.__call__("haha") #也可以这样调用

def test():
	print("test")
	
test.__call__()  #函数也可以这样调用
```
- `__enter__`：方法
- `__exit__`：方法
- `__repr__`：方法，print打印时调用的方法
重写后，可以实现对象打印后想要呈现的内容
```
class Test():
	def __repr__(self):
		return "haha"
t = Test()
print(t) #haha
```
- `__del__`: 方法，对象销毁时调用，和init相反
> 正常情况下，python会自动垃圾回收，但有时可以手动释放对象控件
> 当对象索引为0时，python就会回收该对象；del方法就是删除索引的，索引-1

```
class Father():
	def __del__(self):
		print("del father")

class Son(Father):
	def __del__(self):
		print("del son")
		super().__del__() #如果不调用父类，则不会继承父类的部分不会销毁掉
		
s = Son()  #对象创建时，Son索引为1
# s1 = s    #如果这里有赋值，则索引为2，此时del s就会使索引-1，但因为不为0，不触发销毁
del s   #销毁对象索引 ，Son索引为0，会触发__del__函数
```
- `__dir__`: 方法，查询对象内部所有属性
**注意**：
	常用的是dir();  dir方法会对内部属性进行排序
	`__dir__`的方法必须要传实例，如果传的是类可能得不到你期望的内容，此时要用dir
> 混淆概念
> 实例.__dir__() == dir(实例)
> 类.__dir__(类) != dir(类)   #注意这里
> 类.__dir__(实例) == dir(实例)
> 实例.__dir__() == dir(类)
```
class Father():
	def test():
		pass

f = Father()
# dir(f) == dir(Father) == f.__dir__() == Father.__dir__(f) != Father.__dir__(Father) 这里的==不是真实的，以为list顺序是不一样的，这里只是用来比喻

```


## 部分语法深剖
- `with...as..`
[[#^a6117d|with as 代码语法]]
-- with进入上下文
-- as返回上下文中`__enter__`返回的对象，如无as语句，则不处理`__enter__`的返回对象
- `try...except...else...finally`
**try**: 捕获异常代码块，应尽量精简
**except**: 异常处理代码块，应指定捕捉的异常，否则不应直接使用，防止因此真实的bug
**else**: 非异常时执行的代码块，必须和except同时使用。和try作用域相同，可以直接使用内部变量；且会在finally之前执行；目的是为了精简try代码块，防止冗余；
**finally**: 最终处理模块，不管前面结果如何，都会执行



