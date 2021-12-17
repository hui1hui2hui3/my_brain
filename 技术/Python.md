---
tag: ["Python"]
---

## 高阶函数
- map
- reduce
- filter

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
- `__new__`
- `__init__`
- `__call__` ^764744
- `__enter__`
- `__exit__`

## 部分语法深剖
- `with...as..`
[[#^a6117d|with as 代码语法]]
-- with进入上下文
-- as返回上下文中`__enter__`返回的对象，如无as语句，则不处理`__enter__`的返回对象
- `try...except...else...finally`

