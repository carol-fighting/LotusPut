# 简短 
- 解决了开发中`基础性`的问题简短我解决解决纠结，专注于`应用程序`的开发
- 已集成了20多个模块：核心容器、面向切面、数据访问
- 依赖注入使得构造器和javaBean文件中的依赖关系清晰
- 没有闭门造车，利用了已有的技术、如ORM框架、Logging框架

----------


# 内部容器

		/**定位资源*/
	    Resource resource = new FileSystemResource
		    (new File("src/main/resources/conf/spring-mybatis.xml"));
	    BeanFactory beanFactory = new XmlBeanFactory(resource);
	    Person jap=(Person)beanFactory.getBean("japanese");
	    jap.useAxe();
	    
- `org.springframework.beans`包中的BeanFactory接口,提供了IoC容器最基本功能

----------
# 外部容器 
    ApplicationContext context=new ClassPathXmlApplicationContext
		  (new String[] {"classpath:conf/spring-mybatis.xml" });

 - `org.springframework.context`包下的ApplicationContext接口扩展了BeanFactory
	 - 提供与Spring AOP集成、国际化处理、事件传播
	 - 提供不同层次的context实现 (如针对web应用的WebApplicationContext)
	 - 有了ApplicationContext,可以使用`声明式`Ioc了，与配置文件更兼容 ，之前是`编程式`

----------
# 启动容器**入口**
    <listener>
        <listener-class> org.springframework.web.context.ContextLoaderListener </listener-class>
    </listener>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:conf/spring-mybatis.xml</param-value>
    </context-param>

- Spring监听器:Deployed Resources 中WEB-INF下`web.xml`中的Listener
	 - `ContextLoaderListener`类，一个实现了`ServletContextListener`接口的监听器
	 - 在启动项目时会触发contextInitialized()方法,`创建ApplicationContext对象`

----------
#IoC的加载
- refresh()函数在`AbstractApplicationContext`类中,是`ApplicationContext`的抽象实现类
			
    - 1）定位资源:-Resource
    - 2）载入bean定义:-BeanDefinition
    - 3）IoC中注册Bean:-HashMap持有一系列的BeanDefinition

----------
# 依赖关系注入
- 若设置 lazy-init=false，即设置成`不进行延迟初始化`,则注册bean时触发依赖注入

- 依赖注入的触发是在用户`第一次`向IoC容器索要Bean的时候
	 - 1）CreateBeanInstance():使用`静态工厂`方法，生成java实例对象
	 - 2）PopulateBean():对bean属性的依赖注入进行处理，即对`已经创建的bean实例`进行依赖注入


----------
# bean循环依赖
- 构造器注入：无法解决
- 设值注入：
	- **singleton**：容器`提前暴露`一个singleton正在`创建中`的bean
	- **prototype**:无法解决
	
----------
#  bean生命周期 
- 1）**singleton**：默认是单例的，每次请求该Bean都将获得相同的实例
	- 容器负责`跟踪`Bean实例的`状态`，负责维护Bean实例的生命周期行为
- 2）**prototype**：程序每次请求该id的Bean，Spring都会新建一个Bean实例，然后返回给程序
	- Spring容器仅仅使用new 关键字创建Bean实例
	- 一旦创建成功，容器不再跟踪实例，也不会维护Bean实例的状态。

- 3）WebApplication中还有三种
    - request、session、global session

----------

# **IoC**控制反转
 - 一个重要的面向对象编程的法则：用于削减程序的耦合问题
 - 一个全新的设计模式、但是理论和时间成熟相对较晚，没有被GoF包含
 - 两种类型
	 - （1）依赖注入`DI` dependency Injection
	 - （1）依赖查找`DL` dependency Looup
 - 依赖注入
    -  将耦合从代码中移出去， 放入XML ，容器在需要时形成依赖关系
    - 把以前在工厂方法里写死的`对象生成代码`，改变为由`XML`文件来定义，根据类名`反射`生成对象
	- 把`工厂`和`对象生成`这两者独立分隔开来，目的就是提高灵活性和可维护性。	 

----------

# 自动装配

    <bean id="chinese" class="com.hk.extra.Chinese" autowire="byType">
		
 - autowird 字段值 
    - no： 默认，通过`ref`属性，设置用于装配的目标bean
    - byName：设值注入，寻找与属性名字相同的bean
    - byType：设值注入，寻找与属性类型相同的bean，UnsatisfiedDependencyException
    - constructor：构造器注入，根据构造函数的参数类型，`byType`
    - autodetect：constructor+ byType
  
 - 隐式地向Spring容器注册4个`bean处理器`，识别注解:`<context:annotation-config/>`
	- 1）**@Autowired**： AutowiredAnnotationBeanPostProcessor 
	- 2）**@Resource、@PostConstruct、@PreDestroy**：CommonAnnotationBeanPostProcessor
	- 3）**@PersistenceContext**： PersistenceAnnotationBeanPostProcessor 
	- 4） **@Required**：RequiredAnnotationBeanPostProcessor 
 - 将@Component的类注册成bean,包含前者功能: `<context:component-scan base-package="com.hk" />` 

----------
#   控制反转
 - 程序在`运行过程`中，需要另一个对象的协助时
	 - `无需`代码中`创建被调用者的实例`，而是依赖于外部的注入
	 - 依赖的建立时间推后，从`编译时`推迟到`运行时`

----------


 - 1）伞 a=new 伞(); 调用者自己创建被调用者实例
	 - 被调用的类出现在调用者的代码，`无法`实现两者的松耦合
 - 2）伞 a=factory.create();调用者通过工厂模式，定位具体工厂，获得被调用者实例
	 - 找一个厂，买伞，调用者和被调用者`解耦`、调用者与`特定工厂耦合`在一起
 - 3）：`@autowired`指令 `伞 a`，系统自动提供被调用者的实例
	 - 在家待着不用动，喊一句，我要伞，系统就给伞，无需定位工 
	 - 程序运行到需要被调用者时，系统自动提供被调用者的实例
	 - 调用者和被调用者都处于Spring的管理下，二者之间的依赖关系由Spring提供

----------


# 依赖注入
- 人`Person`用斧子`Axe`
- 具体的`斧子实现类`：石头斧子 `StoneAxe`
- 具体的人持有斧子`Axe`的引用，注入斧子、具体人跟斧子解耦( 不关心`斧子的实现类`、`谁实例化斧子`)
	- 构造器注入`Japanese`
	- 设值注入`Chinese`
	

----------


1）抽象斧头`Axe`

    public interface Axe{
    	public String chop(); 
    }

2）抽象人`Person`

    public interface Person{
    	public void useAxe();  
    }

1-1）具体斧头`StoneAxe`

    public class StoneAxe implements Axe{
    	@Override
    	public String chop(){
    		System.out.println("我是个石头斧子，我砍柴很慢的");
    		return "一个萌萌的石头斧子";
    	}
    }

2-1）具体人：构造器注入Japanese


    public class Japanese implements Person{
    	private Axe axe; 
        public Japanese(Axe axe){  
            this.axe = axe;  
        } 
    	public void useAxe(){
    		System.out.println(axe.chop());
    	}
    }
    
2-2）具体人：设值注入Chinese

    public class Chinese implements Person{
    	private String name;
    	public void setName(String name){
    		this.name = name;
    	}
    
    	private Axe axe;
    	public void setAxe(Axe axe){
    		this.axe = axe;
    	}
    	public void useAxe(){
    		System.out.println("我是" + name + ",用" + axe.chop());
    	}
    }

3）XML文件配置bean注入

    <bean id="stoneAxe" class="com.hk.extra.StoneAxe"/>  
    
	<bean id="japanese" class="com.hk.extra.Japanese"> 
	    <constructor-arg ref="stoneAxe"/>  
	</bean>  

    <bean id="chinese" class="com.hk.extra.Chinese">  
	    <property name="axe" ref="stoneAxe" />  
		<property name="name" value="奋斗牛小二"/>  
	</bean>  

4）在容器context中进行getBean()

    public class BeanTest{
    	static ApplicationContext context = new ClassPathXmlApplicationContext
    			  (new String[] {"classpath:conf/spring-mybatis.xml"});
    	public static void main(String[] args){
    		Person jap=(Person)context.getBean("japanese");
    	    jap.useAxe();
            Person ch = (Person)context.getBean("chinese");  
            ch.useAxe();
    	}
    }

----------