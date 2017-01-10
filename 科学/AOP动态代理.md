# AOP术语
 - **连接点**：被我们拦截到的那个方法
 - 切面的**切点**：指定希望在程序流中拦截的连接点
 - 切面的**通知类型**：
    - 1）前置：在连接点之前
    - 2）后置：在连接点正常完成之后
    - 3）异常：在方法抛出异常退出时
    - 4）最终：2）后置 + 3）异常
    - 5）环绕：可改变参数和返回结果,容易忘记执行原始的方法，invocation.proceed()
         

----------


#基于注解

 - 1）定义切面：MarinlogService接口，是一个注解
 - MarinlogUser类，使用@Aspect修饰
    - 切入点和切入类型 
 - 连接点：
    - MarinlogUser类中 
    
----------
# 代理
    public static void main(String[] args){
    	RealPerson xiaoMing=new RealPerson();
    	ProxyPerson  proxy=new ProxyPerson(xiaoMing);
    		proxy.sentGigt(1);
    }

    public interface abstractPerson {
    	public void sentGigt(int toPersonId);
    }
    public class RealPerson implements abstractPerson{
    	@Override
    	public void sentGigt(int toPersonId){
    		Print.print(" 被代理的类");		
    	}
	}
    public class ProxyPerson implements abstractPerson{
    	public RealPerson real;
    	public ProxyPerson(RealPerson real){
    		this.real=real;
    	}
    	@Override
    	public void sentGigt(int toPersonId){
    		real.sentGigt(toPersonId);
    	}
    }
    
 -  静态代理：与反射无关，运行前，代理类的class 文件已生成，编译期间进行**静态织入**
 -  动态代理：反射生成代理类的字节码
    - 定义proxyHandler,实现InvocationHandler接口，持有被代理接口的引用，重写invoke()
    - Proxy.newProxyInstance(**加载器，接口列表，proxyHandler**)生成代理类对象，继承了Proxy类，实现了被代理者的所有接口，final
 - cglib（代理类）
    - 若被代理者没有实现任何接口，创建cglib代理，使用ASM（java字节码处理框架）
    - 可强制    < aop:aspectj-autoproxy proxy-target-class="true"/>
          
----------


#AOP介绍
- AOP是对OOP思想的补充和完善，OOP引进"抽象"、"封装"、"继承"、"多态"等概念，进行**由上到下**的抽象和**层次**封装
	 - 但是具体细粒度到每个事物内部的情况，OOP就显得无能为力了，比如日志功能。
	 - 日志代码往往**水平地**散布在所有**对象层次**当中，与它所散布到的**对象的核心功能**无关。
	 - 导致了大量代码的重复，不利于各个模块的重用

- **横切技术**
	 -  剖解开封装的对象内部，将**公共行为**封装成一个独立的模块（切面）
	 - 又能将剖开的切面复原，融入核心业务逻辑中。这样，能便于之后横切功能的编辑和重用
	 
----------

----------