# ��� 
- ����˿�����`������`���������ҽ��������ᣬרע��`Ӧ�ó���`�Ŀ���
- �Ѽ�����20���ģ�飺�����������������桢���ݷ���
- ����ע��ʹ�ù�������javaBean�ļ��е�������ϵ����
- û�б����쳵�����������еļ�������ORM��ܡ�Logging���

----------


# �ڲ�����

		/**��λ��Դ*/
	    Resource resource = new FileSystemResource
		    (new File("src/main/resources/conf/spring-mybatis.xml"));
	    BeanFactory beanFactory = new XmlBeanFactory(resource);
	    Person jap=(Person)beanFactory.getBean("japanese");
	    jap.useAxe();
	    
- `org.springframework.beans`���е�BeanFactory�ӿ�,�ṩ��IoC�������������

----------
# �ⲿ���� 
    ApplicationContext context=new ClassPathXmlApplicationContext
		  (new String[] {"classpath:conf/spring-mybatis.xml" });

 - `org.springframework.context`���µ�ApplicationContext�ӿ���չ��BeanFactory
	 - �ṩ��Spring AOP���ɡ����ʻ������¼�����
	 - �ṩ��ͬ��ε�contextʵ�� (�����webӦ�õ�WebApplicationContext)
	 - ����ApplicationContext,����ʹ��`����ʽ`Ioc�ˣ��������ļ������� ��֮ǰ��`���ʽ`

----------
# ��������**���**
    <listener>
        <listener-class> org.springframework.web.context.ContextLoaderListener </listener-class>
    </listener>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:conf/spring-mybatis.xml</param-value>
    </context-param>

- Spring������:Deployed Resources ��WEB-INF��`web.xml`�е�Listener
	 - `ContextLoaderListener`�࣬һ��ʵ����`ServletContextListener`�ӿڵļ�����
	 - ��������Ŀʱ�ᴥ��contextInitialized()����,`����ApplicationContext����`

----------
#IoC�ļ���
- refresh()������`AbstractApplicationContext`����,��`ApplicationContext`�ĳ���ʵ����
			
    - 1����λ��Դ:-Resource
    - 2������bean����:-BeanDefinition
    - 3��IoC��ע��Bean:-HashMap����һϵ�е�BeanDefinition

----------
# ������ϵע��
- ������ lazy-init=false�������ó�`�������ӳٳ�ʼ��`,��ע��beanʱ��������ע��

- ����ע��Ĵ��������û�`��һ��`��IoC������ҪBean��ʱ��
	 - 1��CreateBeanInstance():ʹ��`��̬����`����������javaʵ������
	 - 2��PopulateBean():��bean���Ե�����ע����д�������`�Ѿ�������beanʵ��`��������ע��


----------
# beanѭ������
- ������ע�룺�޷����
- ��ֵע�룺
	- **singleton**������`��ǰ��¶`һ��singleton����`������`��bean
	- **prototype**:�޷����
	
----------
#  bean�������� 
- 1��**singleton**��Ĭ���ǵ����ģ�ÿ�������Bean���������ͬ��ʵ��
	- ��������`����`Beanʵ����`״̬`������ά��Beanʵ��������������Ϊ
- 2��**prototype**������ÿ�������id��Bean��Spring�����½�һ��Beanʵ����Ȼ�󷵻ظ�����
	- Spring��������ʹ��new �ؼ��ִ���Beanʵ��
	- һ�������ɹ����������ٸ���ʵ����Ҳ����ά��Beanʵ����״̬��

- 3��WebApplication�л�������
    - request��session��global session

----------

# **IoC**���Ʒ�ת
 - һ����Ҫ����������̵ķ�����������������������
 - һ��ȫ�µ����ģʽ���������ۺ�ʱ�������Խ���û�б�GoF����
 - ��������
	 - ��1������ע��`DI` dependency Injection
	 - ��1����������`DL` dependency Looup
 - ����ע��
    -  ����ϴӴ������Ƴ�ȥ�� ����XML ����������Ҫʱ�γ�������ϵ
    - ����ǰ�ڹ���������д����`�������ɴ���`���ı�Ϊ��`XML`�ļ������壬��������`����`���ɶ���
	- ��`����`��`��������`�����߶����ָ�������Ŀ�ľ����������ԺͿ�ά���ԡ�	 

----------

# �Զ�װ��

    <bean id="chinese" class="com.hk.extra.Chinese" autowire="byType">
		
 - autowird �ֶ�ֵ 
    - no�� Ĭ�ϣ�ͨ��`ref`���ԣ���������װ���Ŀ��bean
    - byName����ֵע�룬Ѱ��������������ͬ��bean
    - byType����ֵע�룬Ѱ��������������ͬ��bean��UnsatisfiedDependencyException
    - constructor��������ע�룬���ݹ��캯���Ĳ������ͣ�`byType`
    - autodetect��constructor+ byType
  
 - ��ʽ����Spring����ע��4��`bean������`��ʶ��ע��:`<context:annotation-config/>`
	- 1��**@Autowired**�� AutowiredAnnotationBeanPostProcessor 
	- 2��**@Resource��@PostConstruct��@PreDestroy**��CommonAnnotationBeanPostProcessor
	- 3��**@PersistenceContext**�� PersistenceAnnotationBeanPostProcessor 
	- 4�� **@Required**��RequiredAnnotationBeanPostProcessor 
 - ��@Component����ע���bean,����ǰ�߹���: `<context:component-scan base-package="com.hk" />` 

----------
#   ���Ʒ�ת
 - ������`���й���`�У���Ҫ��һ�������Э��ʱ
	 - `����`������`�����������ߵ�ʵ��`�������������ⲿ��ע��
	 - �����Ľ���ʱ���ƺ󣬴�`����ʱ`�Ƴٵ�`����ʱ`

----------


 - 1��ɡ a=new ɡ(); �������Լ�������������ʵ��
	 - �����õ�������ڵ����ߵĴ��룬`�޷�`ʵ�����ߵ������
 - 2��ɡ a=factory.create();������ͨ������ģʽ����λ���幤������ñ�������ʵ��
	 - ��һ��������ɡ�������ߺͱ�������`����`����������`�ض��������`��һ��
 - 3����`@autowired`ָ�� `ɡ a`��ϵͳ�Զ��ṩ�������ߵ�ʵ��
	 - �ڼҴ��Ų��ö�����һ�䣬��Ҫɡ��ϵͳ�͸�ɡ�����趨λ�� 
	 - �������е���Ҫ��������ʱ��ϵͳ�Զ��ṩ�������ߵ�ʵ��
	 - �����ߺͱ������߶�����Spring�Ĺ����£�����֮���������ϵ��Spring�ṩ

----------


# ����ע��
- ��`Person`�ø���`Axe`
- �����`����ʵ����`��ʯͷ���� `StoneAxe`
- ������˳��и���`Axe`�����ã�ע�븫�ӡ������˸����ӽ���( ������`���ӵ�ʵ����`��`˭ʵ��������`)
	- ������ע��`Japanese`
	- ��ֵע��`Chinese`
	

----------


1������ͷ`Axe`

    public interface Axe{
    	public String chop(); 
    }

2��������`Person`

    public interface Person{
    	public void useAxe();  
    }

1-1�����師ͷ`StoneAxe`

    public class StoneAxe implements Axe{
    	@Override
    	public String chop(){
    		System.out.println("���Ǹ�ʯͷ���ӣ��ҿ��������");
    		return "һ�����ȵ�ʯͷ����";
    	}
    }

2-1�������ˣ�������ע��Japanese


    public class Japanese implements Person{
    	private Axe axe; 
        public Japanese(Axe axe){  
            this.axe = axe;  
        } 
    	public void useAxe(){
    		System.out.println(axe.chop());
    	}
    }
    
2-2�������ˣ���ֵע��Chinese

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
    		System.out.println("����" + name + ",��" + axe.chop());
    	}
    }

3��XML�ļ�����beanע��

    <bean id="stoneAxe" class="com.hk.extra.StoneAxe"/>  
    
	<bean id="japanese" class="com.hk.extra.Japanese"> 
	    <constructor-arg ref="stoneAxe"/>  
	</bean>  

    <bean id="chinese" class="com.hk.extra.Chinese">  
	    <property name="axe" ref="stoneAxe" />  
		<property name="name" value="�ܶ�ţС��"/>  
	</bean>  

4��������context�н���getBean()

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