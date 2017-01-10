# 双亲委派机制

 - 类加载器：引导、拓展、系统
 - 获取系统类加载器ClassLoader.getSystemClassLoader()
 - 实现自己的类加载器，继承java.lang.ClassLoader，父类：该类的类加载器
 - 类都维护着一个指向**定义该类的类加载器**的引用，getClassLoader()
 - loadClass()：封装了代理模式，检查类是否已被加载，调用父类的loadClass()，
 - 父类无法加载，调用findClass()，只重写findClass不会破坏双亲委派，要重写loadClass()
        
    
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
# 类的生命周期
 - 1）加载：由全限定名查找class文件，转为方法区运行时数据结构，生成Class对象
 - 2）链接：校验、准备（**静态变量初始化**）、解析（字段方法名 符号引转直引）
 - 3）初始化（主引）：new、反射、main()、static()、static变量get/set、子类即父类
 - 4） 卸载：**方法区**清空类信息，堆中无类的实例、ClassLoader已回收、Class对象无法被引用、反射

----------
# 类的初始化顺序

- 父类的**静态成员**、 父类的**静态初始化语句**
- 子类的静态成员、 子类的静态初始化语句
- 父类的**成员**、父类的**初始化语句**、父类的**构造函数**
- 子类的成员、子类的初始化语句、子类的构造函数

----------


# 6大区域+直接内存

 - 1）**程序计数器**：各线程记录下一条字节码指令
 - 2）**java栈**：各线程每运行一个方法，创建一个栈帧
 - 3）**本地方法栈**
<br>
 - 1）**方法区**:类结构信息
 - 2）**运行时常量池**：每个calss文件的常量表
 - 3）**堆**
    - 年轻代：复制清理CC
    - 老年代：标记整理MM
    - 永久代：

----------
# 内存区域参数

 - 堆初始值=Xms；堆最大值=Xmx
 - 新生代大小=Xmn；每个线程栈大小=Xss
 - 新生代比值=XX:SurvivorRatio Eden:Survivor 8:1:1
 - 永久代初始值=XX:PermSize；永久代最大值：XX:MaxPermSize
 - 直接升老年代的对象大小=XX:PretenureSizeThreshold

----------
# 垃圾回收
 - 1）对谁进行GC：堆和方法区的内存
 - 2）何时进行GC：从root搜索不可到达的对象，且经过第一次标记、清理后，仍然没有复活的对象
 - 3）判断对象存活：引用计数算法、根搜索算法 (可达性分析算法)
    - gc roots： （虚拟机栈、本地方法栈、方法区的类静态属性、常量）引用的对象
 -  4）GC有环怎么办：引用计数算法对此不适用，但是可达性分析算法适用
 -  5）如何逃脱GC：重写finalize(),逃脱一次。没有重写或已调用，则无效
 -  6）永久代如何GC：永久代和老年代的垃圾回收是绑定的，其中一个占满，两个区都进行垃圾回收
 -  7）新生代gc、检查老年代是否引用新生代的对象：
    - 老年代中维护一个512 byte的块"card table"，所有老年代对象引用新生代对象的记录

----------
# 内存泄漏
 - 例子：vector.add(obj);obj=null;
 - 性能调优：线程池和JVM启动参数
 - 现象：GC进行时间变长、full GC次数变多、老年代内存变大
 - DUMP定位：jmap：内存：栈快照：二进制；jstack：CPU：线程位置：文本
 - 一块内存、符合垃圾收集器进行收集的标准
    - 对象赋予了null,并再也没调用过该对象
    - 对象已赋予了新值，即重新分配了内存空间
 - 引起内存泄漏的几个原因：
    - 1）static集合类,Vector、HashMap等，静态遍历的生命周期与应用程序一致
    - 2）监听器，addListener()，但是释放对象的时候没有删除这些监听器
	 
    - 3）物理连接，如数据库连接、网络连接，独立于JVM，需显示关闭
		 - DataSource.getConnection()创建，close()，自动Resultset 和Statement对象为NULL
		 - 连接池，close()，需要显式地关闭Resultset、Statement 对象其中一个
    - 4）内部类、外部模块的引用
	 - 模块B调用了模块A的一个方法，传入了一个对象给模块A，
	 - 模块A保持了对该对象的引用，模块A应提供相应的操作去除引用

----------