---
tag: ["Python","Selenium"]
---
基于[[Python]]+[[Selenium]]
## 框架分层
> 总共可分为四层，组件层、逻辑层、业务层、数据层；有时又会只写三层、页面逻辑层、业务层、数据层；有时会只写2层，页面逻辑层，数据业务层；**常用的是三层结构**
- 组件层：(又称PageObject)
> 对页面的元素进行封装，主要包含定位符和元素基本操作；当然还可以继续拆分组件，提取出公共组件，比如InputElement,ButtonElement,UploadElement等公共元素

例子
```Python
class LoginPage(BasePage):
	login_loc = (By.XPATH,"//button[text()='登录']")
	name_loc = (By.XPATH,"//input[@placeholder='姓名']")
	pawd_loc = (By.XPATH,"//input[@placeholder='密码']")
	
	def open(driver):
		driver.get('/login')
	
	def input_name(driver,name):
		#这里是简单的例子而已；实际应当采用隐含等待方法；比如WebDriverWait系列
		driver.find_element(*name_loc).send_keys(name)
	
	def input_pawd(driver,pawd):
		#这里是简单的例子而已；实际应当采用隐含等待方法；比如WebDriverWait系列
		driver.find_element(*pawd_loc).send_keys(pawd)
	
	def click_login(driver):
		#这里是简单的例子而已；实际应当采用隐含等待方法；比如WebDriverWait系列
		driver.find_element(*login_loc).click()
```

解释：**为什么要把loc放到Page的顶部，因为这个可以快速判断当前页面的元素是否正确或是否有遗漏；还有一种写法是把所有loc全部抽取出去放到一个文件夹下，方便维护**
- 逻辑层
> 对单个页面的元素基本操作进行封装，有时可以和组件层合并到一层

例子
```Python
class LoginLogic(object):

	def login(driver,name,pawd):
		LoginPage.open()
		LoginPage.input_name(driver,name)
		LoginPage.input_pawd(driver,pawd)
		LoginPage.login(driver)
```
- 业务层
> 对一整套业务流程进行封装

例子
```Python
class LoginBusiness(object):
	
	def login(driver,name,pawd):
		LoginLogic.login(driver,name,pawd)

```

- 数据层
> 对业务层提供具体数据驱动，有时又会和业务层合并到同一层

例子
```Python

class TestLoginPage(object):

	def test_login_normal():
		LoginBusiness.login(driver,"name","pqwd")

```

## 框架扩展
### 基于[[OpenCV]]实现图像识别的页面操作
**技术包括**
- [[PyMouse]] 鼠标控制
- [[Pillow]]      图像处理，屏幕截屏
- [[PyKeyboard]]  键盘操作
- [[OpenCV]]  图像识别

**实现思路**
1. 截取目标图像
2. 框架建立字段和目标图像的关系，通过装饰器和反射
3. 实现基础操作方法通过pymouse和pykeyboard和pillow和opencv，实现包括click,input,hold等方法
	1. 触发事件是，通过pillow自动截取当前屏幕图	
	2. 通过opencv识别到目标图像在屏幕截图的位置
	3. 通过屏幕像素和鼠标像素，计算坐标系比例
	4. 通过比率，和坐标位置，计算出真实鼠标坐标
	5. 执行对应操作
4. 建立目标元素和操作方法的关系，通过装饰器和反射

## 框架优劣势
### 优势
### 劣势
### 改进计划

