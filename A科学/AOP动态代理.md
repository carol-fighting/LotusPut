# AOP����
 - **���ӵ�**�����������ص����Ǹ�����
 - �����**�е�**��ָ��ϣ���ڳ����������ص����ӵ�
 - �����**֪ͨ����**��
    - 1��ǰ�ã������ӵ�֮ǰ
    - 2�����ã������ӵ��������֮��
    - 3���쳣���ڷ����׳��쳣�˳�ʱ
    - 4�����գ�2������ + 3���쳣
    - 5�����ƣ��ɸı�����ͷ��ؽ��,��������ִ��ԭʼ�ķ�����invocation.proceed()
         

----------


#����ע��

 - 1���������棺MarinlogService�ӿڣ���һ��ע��
 - MarinlogUser�࣬ʹ��@Aspect����
    - �������������� 
 - ���ӵ㣺
    - MarinlogUser���� 
    
----------
# ����
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
    		Print.print(" ���������");		
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
    
 -  ��̬�����뷴���޹أ�����ǰ���������class �ļ������ɣ������ڼ����**��̬֯��**
 -  ��̬�����������ɴ�������ֽ���
    - ����proxyHandler,ʵ��InvocationHandler�ӿڣ����б�����ӿڵ����ã���дinvoke()
    - Proxy.newProxyInstance(**���������ӿ��б�proxyHandler**)���ɴ�������󣬼̳���Proxy�࣬ʵ���˱������ߵ����нӿڣ�final
 - cglib�������ࣩ
    - ����������û��ʵ���κνӿڣ�����cglib����ʹ��ASM��java�ֽ��봦���ܣ�
    - ��ǿ��    < aop:aspectj-autoproxy proxy-target-class="true"/>
          
----------


#AOP����
- AOP�Ƕ�OOP˼��Ĳ�������ƣ�OOP����"����"��"��װ"��"�̳�"��"��̬"�ȸ������**���ϵ���**�ĳ����**���**��װ
	 - ���Ǿ���ϸ���ȵ�ÿ�������ڲ��������OOP���Ե�����Ϊ���ˣ�������־���ܡ�
	 - ��־��������**ˮƽ��**ɢ��������**������**���У�������ɢ������**����ĺ��Ĺ���**�޹ء�
	 - �����˴���������ظ��������ڸ���ģ�������

- **���м���**
	 -  �ʽ⿪��װ�Ķ����ڲ�����**������Ϊ**��װ��һ��������ģ�飨���棩
	 - ���ܽ��ʿ������渴ԭ���������ҵ���߼��С��������ܱ���֮����й��ܵı༭������
	 
----------

----------