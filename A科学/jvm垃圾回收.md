# ˫��ί�ɻ���

 - �����������������չ��ϵͳ
 - ��ȡϵͳ�������ClassLoader.getSystemClassLoader()
 - ʵ���Լ�������������̳�java.lang.ClassLoader�����ࣺ������������
 - �඼ά����һ��ָ��**���������������**�����ã�getClassLoader()
 - loadClass()����װ�˴���ģʽ��������Ƿ��ѱ����أ����ø����loadClass()��
 - �����޷����أ�����findClass()��ֻ��дfindClass�����ƻ�˫��ί�ɣ�Ҫ��дloadClass()
        
    
      protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
          synchronized (getClassLoadingLock(name)) {
                Class<?> c = findLoadedClass(name);
                if (c == null) {
                    long t0 = System.nanoTime();
                    try {
                        if (parent != null) {
                            c = parent.loadClass(name, false);
                        }
                        else {
                            c = findBootstrapClassOrNull(name);
                        }
                    }
                    catch (ClassNotFoundException e) {
                    }
                    if (c == null) {
                        long t1 = System.nanoTime();
                        c = findClass(name);
                        sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                        sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                        sun.misc.PerfCounter.getFindClasses().increment();
                    }
                }
                if (resolve) {
                    resolveClass(c);
                }
                return c;
            }
        }

----------
# �����������
 - 1�����أ���ȫ�޶�������class�ļ���תΪ����������ʱ���ݽṹ������Class����
 - 2�����ӣ�У�顢׼����**��̬������ʼ��**�����������ֶη����� ������תֱ����
 - 3����ʼ������������new�����䡢main()��static()��static����get/set�����༴����
 - 4�� ж�أ�**������**�������Ϣ�����������ʵ����ClassLoader�ѻ��ա�Class�����޷������á�����

----------
# ��ĳ�ʼ��˳��

- �����**��̬��Ա**�� �����**��̬��ʼ�����**
- ����ľ�̬��Ա�� ����ľ�̬��ʼ�����
- �����**��Ա**�������**��ʼ�����**�������**���캯��**
- ����ĳ�Ա������ĳ�ʼ����䡢����Ĺ��캯��

----------


# 6������+ֱ���ڴ�

 - 1��**���������**�����̼߳�¼��һ���ֽ���ָ��
 - 2��**javaջ**�����߳�ÿ����һ������������һ��ջ֡
 - 3��**���ط���ջ**
<br>
 - 1��**������**:��ṹ��Ϣ
 - 2��**����ʱ������**��ÿ��calss�ļ��ĳ�����
 - 3��**��**
    - ���������������CC
    - ��������������MM
    - ���ô���

----------
# �ڴ��������

 - �ѳ�ʼֵ=Xms�������ֵ=Xmx
 - ��������С=Xmn��ÿ���߳�ջ��С=Xss
 - ��������ֵ=XX:SurvivorRatio Eden:Survivor 8:1:1
 - ���ô���ʼֵ=XX:PermSize�����ô����ֵ��XX:MaxPermSize
 - ֱ����������Ķ����С=XX:PretenureSizeThreshold

----------
# ��������
 - 1����˭����GC���Ѻͷ��������ڴ�
 - 2����ʱ����GC����root�������ɵ���Ķ����Ҿ�����һ�α�ǡ��������Ȼû�и���Ķ���
 - 3���ж϶�������ü����㷨���������㷨 (�ɴ��Է����㷨)
    - gc roots�� �������ջ�����ط���ջ�����������ྲ̬���ԡ����������õĶ���
 -  4��GC�л���ô�죺���ü����㷨�Դ˲����ã����ǿɴ��Է����㷨����
 -  5���������GC����дfinalize(),����һ�Ρ�û����д���ѵ��ã�����Ч
 -  6�����ô����GC�����ô�������������������ǰ󶨵ģ�����һ��ռ������������������������
 -  7��������gc�����������Ƿ������������Ķ���
    - �������ά��һ��512 byte�Ŀ�"card table"�����������������������������ļ�¼

----------
# �ڴ�й©
 - ���ӣ�vector.add(obj);obj=null;
 - ���ܵ��ţ��̳߳غ�JVM��������
 - ����GC����ʱ��䳤��full GC������ࡢ������ڴ���
 - DUMP��λ��jmap���ڴ棺ջ���գ������ƣ�jstack��CPU���߳�λ�ã��ı�
 - һ���ڴ桢���������ռ��������ռ��ı�׼
    - ��������null,����Ҳû���ù��ö���
    - �����Ѹ�������ֵ�������·������ڴ�ռ�
 - �����ڴ�й©�ļ���ԭ��
    - 1��static������,Vector��HashMap�ȣ���̬����������������Ӧ�ó���һ��
    - 2����������addListener()�������ͷŶ����ʱ��û��ɾ����Щ������
	 
    - 3���������ӣ������ݿ����ӡ��������ӣ�������JVM������ʾ�ر�
		 - DataSource.getConnection()������close()���Զ�Resultset ��Statement����ΪNULL
		 - ���ӳأ�close()����Ҫ��ʽ�عر�Resultset��Statement ��������һ��
    - 4���ڲ��ࡢ�ⲿģ�������
	 - ģ��B������ģ��A��һ��������������һ�������ģ��A��
	 - ģ��A�����˶Ըö�������ã�ģ��AӦ�ṩ��Ӧ�Ĳ���ȥ������

----------