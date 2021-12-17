---
tag: ["异步","Python","协程"]
---
## async
可单独使用，定义一个协程任务
```
async def test():
	print("start")
```
协程必须await才能启动，且内部无await时，会直接执行完毕，不会中途切出
## await
必须要在async内部使用
```
async def test():
	print("start")
	await asyncio.sleep(1)
	print("end")
```
await 启动后，会停在内部的await，然后切出，等待sleep结束，后会切回继续执行
## asyncio
扩展包，提供异步协程封装

对比专用：同步调用
```
# 定义一个协程任务1  
async def task1():  
    print("init task1")  
    await asyncio.sleep(2)  
    print("ed task1")  
    return "task1"  
  
  
# 定义一个协程任务2  
async def task2():  
    print("init task2")  
    await asyncio.sleep(1)  
    print("ed task2")  
    return "task2"  
  
  
async def main():  
    taska = task1()  
    taskb = task2()  
    await taskb  
    await taska  
  
  
asyncio.run(main())
```
### create_task
异步调用
```
#定义一个协程任务1
async def task1():
	print("init task1")
	await asyncio.sleep(1)
	print("ed task1")
	return "task1"

#定义一个协程任务2
async def task2():
	print("init task2")
	await asyncio.sleep(1)
	print("ed task2")
	return "task2"

async def main():
	taska = asyncio.create_task(task1())
	taskb = asyncio.create_task(task2())
	await asyncio.sleep(3)
	
asyncio.run(main())
```
### gather
异步调用，有序返回
```
#定义一个协程任务1
async def task1():
	print("init task1")
	await asyncio.sleep(1)
	print("ed task1")
	return "task1"

#定义一个协程任务2
async def task2():
	print("init task2")
	await asyncio.sleep(1)
	print("ed task2")
	return "task2"

async def main():
	results = await asyncio.gather(task1(),task2())
	print(results)

asyncio.run(main())
```
### as_completed
异步调用，无序返回
```
#定义一个协程任务1
async def task1():
	print("init task1")
	await asyncio.sleep(1)
	print("ed task1")
	return "task1"

#定义一个协程任务2
async def task2():
	print("init task2")
	await asyncio.sleep(1)
	print("ed task2")
	return "task2"

async def main():
	for task in asyncio.as_completed([task1(),task2()]):
		result = await task
		print(result)

asyncio.run(main())
```
### wait
异步调用，无序返回
```
#定义一个协程任务1
async def task1():
	print("init task1")
	await asyncio.sleep(1)
	print("ed task1")
	return "task1"

#定义一个协程任务2
async def task2():
	print("init task2")
	await asyncio.sleep(1)
	print("ed task2")
	return "task2"

async def main():
	done,pending = await asyncio.wait([task1(),task2()])
	for task in done:
		print(task.result())
		
asyncio.run(main())
```

## 协程
核心`event loop`时间循环