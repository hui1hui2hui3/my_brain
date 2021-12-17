---
tag: ["Java"]
---

## 面向对象
### 多态
- 什么是多态-What
> 多态就是同一类事务，相同的方法，但是却有不同的表现；
- 为什么要多态-Why
> 为了让调用方，不需要考虑具体的类是哪个，只需知晓其父类即可，正常调用方法；且可以实现不同情况下，展现不同的形态，但上层调用方，却无需改变；更加直接的说法就是**解耦**
- 怎么实现多态-How
> 主要面向的是**继承、实现**

例子：
```Java
	public abstract Class Bird {
		public abstract void singing();
	}
	public Class BaiLing extends Bird {
		public void singing(){
			System.out.print("jiu jiu jiu")
		}
	}
	public Class WuYa extends Bird {
		public void singing(){
			System.out.print("wa wa wa")
		}
	}
	public static void main(){
		Bird niao = new WuYa()
		niao.singing()
		niao = new BaiLing()
		niao.singing()
	}
```
	

### 重载
- 什么是重载-What
> 重载是同一个对象下，相同的方法名称，但却有不同的返回值和入参
- 为什么要重载-Why
> 重载对外部调用者来说，同一个方法，不同入参可以得到不同功能，让调用者不感知方法内部的功能
- 怎么重载-How
> 主要针对**相同的方法名**
例子：
```Java

public class PoBiJi {
	
	public void poBi(Meet meet) {
		System.out.print("碎肉")
	}
	
	public void poBi(Bean bean,Water water) {
		System.out.print("豆浆")
	}
	
	public void poBi(Fruit fruit,Water water) {
		System.out.print("果汁")
	}
	
}

public interface Fruit {
}
public class Orange implements Fruit {
}
public class Apple implements Fruit {
}

public class Test{
	public static void main(String[] args){
		PoBoJi pbj = new PoBoJi()
		pbj.poBi(new Meet())
		pbj.poBi(new Bean(),new Water())
		pbj.poBi(new Apple(),new Water())
	}

}
```
	
## 设计模式
![[单例设计模式]]
## 框架
### Sprint Boot
[[Spring Boot]]