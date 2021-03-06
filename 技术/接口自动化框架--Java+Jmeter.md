---
tag:["Java","Jmeter","数据驱动","接口测试"]
---
## 目标[[接口测试]]
## 使用技术
- [[Java]]
- [[Jmeter]]
- [[Ant]]
- [[Maven]]

## 代码架构
1. maven启动java主方法
2. java扩展jmx文件，增强功能，然后启动ant
3. ant启动具体的jmeter
4. jmeter运行后生产jtl回到java
5. java解析jtl生产报告，并发送邮件

## 实现功能
1. 基于jmeter的xml模板功能，实现jmx功能的扩展
	1. 自动重置环境配置，防止个人配置提交
	2. 自动为接口添加动态标题（包括业务场景和接口名称）
	3. 自动为场景添加失败重试机制
	4. 自动为接口和场景添加[[数据驱动]]机制

2. 扩展功能
	1. 接口查重功能
	2. 变量查重功能
	3. 场景查重功能

3. [[持续集成]]
	1. 使用[[Git]]管理脚本
	2. 使用[[Maven]]启动测试
	3. 在[[Jenkins]]上构建job，定时拉取代码，启动测试

## 实现原理
### 数据驱动
1. 在csv中定义场景-接口-参数
2. xml模板使用whilecontroller，counter和beanshell
3. 根据接口提供参数量，确定循环次数，并给接口打上参数标记，以便在报告中区分

### 失败重试
1. xml模板中使用whilecontroller，counter和beanshell
2. beanshell中判断成功还是失败，失败则标记重试，并给接口打上失败标记以便在报告中区分
3. 根据限制的最大循环尝试次数，进行失败重试

## 遇到问题
### 开发中的问题
1. **变量作用域覆盖问题**，数据驱动变量，自定义变量和paramlized controller以及提取器提取的变量四者之间的变量冲突问题，增加了框架扩展功能变量查重
2. **开发冲突问题**，由于大家经常修改同一个jmx文件，导致文件冲突，且由于是xml格式，解决冲突难
3. **脚本调试问题**，大家不熟悉框架，运行调试起来麻烦，通过培训解决
4.**脚本业务接口不清楚如何编写**，大家不知道如何区分何为关键业务接口，通过培训解决
5. **忘记编写场景名称或接口名称**，一般是复制过来忘记更改了，增加了场景查重和去重功能，框架添加编号

### 运行中的问题
1. **环境不稳定**，调整运行时间段，防止业务冲突；
2. **业务数据并行冲突**，调整运行时间段，防止业务冲突；
	1. 强化版：可以每个环境docker独立容器，独立环境进行测试，防止冲突；
3. **接口场景本身不稳定**，增加失败重试机制，避免大量问题

## 框架优劣势-如何改进
### 优势
1. 测试人员不需要懂java技术，只需要了解jmeter如何使用即可上手录入接口
### 劣势
1. 提供web页面在线编辑jmeter文件，而不是使用jmeter直接编辑

### 劣势改进