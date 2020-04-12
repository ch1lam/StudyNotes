> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://juejin.im/post/5d1efae26fb9a07ea6489355#heading-7

一、什么是 JVM？
==========

JVM 是 Java Virtual Machine（Java 虚拟机）的缩写，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。由**一套字节码指令集、一组寄存器、一个栈、一个垃圾回收堆和一个存储方法域等**组成。JVM 屏蔽了与操作系统平台相关的信息，使得 Java 程序只需要生成在 Java 虚拟机上运行的目标代码（字节码），就可在多种平台上不加修改的运行，这也是 Java 能够 “**一次编译，到处运行的**” 原因。

二、JRE、JDK 和 JVM 的关系
===================

**JRE（Java Runtime Environment， Java 运行环境）**是 Java 平台，所有的程序都要在 JRE 下才能够运行。包括 JVM 和 Java 核心类库和支持文件。  

**JDK（Java Development Kit，Java 开发工具包）**是用来编译、调试 Java 程序的开发工具包。包括 Java 工具（javac/java/jdb 等）和 Java 基础的类库（java API ）。

**JVM（Java Virtual Machine， Java 虚拟机）**是 JRE 的一部分。JVM 主要工作是解释自己的指令集（即字节码）并映射到本地的 CPU 指令集和 OS 的系统调用。Java 语言是跨平台运行的，不同的操作系统会有不同的 JVM 映射规则，使之与操作系统无关，完成跨平台性。

下图表示了 JDK、JRE 和 JVM 三者间的关系：

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fb0ef7ec2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

总结：使用 JDK（调用 JAVA API）开发 JAVA 程序后，通过 JDK 中的编译程序（javac）将 Java 程序编译为 Java 字节码，在 JRE 上运行这些字节码，JVM 会解析并映射到真实操作系统的 CPU 指令集和 OS 的系统调用。

三、JVM 原理
========

## java 体系结构介绍：

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103feeb2e7aa?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Class Loader（类加载器）：用于装载. class 文件。  

Execution Engine（执行引擎）：用于执行字节码或者本地方法。

运行时数据区：方法区、堆、java 栈、pc 寄存器、本地方法栈。

### 1、JVM 生命周期介绍

Java 实例对应一个独立运行的 Java 程序（**进程级别**）

（1）**启动**

启动一个 Java 程序，一个 JVM 实例就产生。拥有 public static void main (String [] args) 函数的 class 可以作为 JVM 实例运行的起点。

（2）**运行**

main () 作为程序初始线程的起点，任何其他线程均可由该线程启动。JVM 内部有两种线程：守护线程和非守护线程，main () 属于非守护线程，守护线程通常由 JVM 使用，程序可以指定创建的线程为守护线程。

（3）**消亡**

当程序中的所有非守护线程都终止时，JVM 才退出；若安全管理器允许，程序也可以使用 Runtime 类或者 System.exit () 来退出。

JVM 执行引擎实例则对应了属于用户运行程序线程它是线程级别的。

### 2、Java 类加载器

#### **Java 加载类的过程**

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fb1124ac7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**（1）装载（loading）**

负责找到二进制字节码并加载至 JVM 中，JVM 通过类名、类所在的包名、ClassLoader 完成类的加载。因此，标识一个被加载了的类：类名 + 包名 + ClassLoader 实例 ID。  

**（2）链接（linking）**

负责对二进制字节码的格式进行校验、初始化装载类中的静态变量以及解析类中调用的接口。完成校验后，JVM 初始化类中的静态变量，并将其赋值为默认值。最后对比类中的所有属性、方法进行验证，以确保要调用的属性、方法存在，以及具备访问权限（例如 private、public 等），否则会造成 NoSuchMethodError、NoSuchFieldError 等错误信息。

**（3）初始化（initializing）**

负责执行类中的静态初始化代码、构造器代码以及静态属性的初始化，以下**四种情况**初始化过程会被触发。

①调用 new

②反射调用了类中的方法

③子类调用了初始化

④JVM 启动过程终止定的初始化类

#### JVM 类加载顺序

##### **（1）层级结构**

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fb72278c0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

①Booststrap ClassLoader  

跟 ClassLoader，C++ 实现，JVM 启动时初始化此 ClassLoader，并由此完成 $JAVA_HONE 中 jre/lib/rt.jar（Sun JDK 的实现）中所有 class 文件的加载，这个 jar 中包含了 java 规范定义的所有接口以及实现。

②Extension ClassLoader

JVM 用此 classloader 来加载扩展功能的一些 jar 包

③System ClassLoader

JVM 用此 ClassLoader 来加载启动参数中指定的 ClassPath 中的 jar 包以及目录，在 Sun JDK 中 ClassLoader 对应的类名为 AppClassLoader。

④User-Defined ClassLoader

User-Defined ClassLoader 是 Java 开发人员继承 ClassLoader 抽象类实现的 ClassLoader，基于自定义的 ClassLoader 可用于加载非 ClassPath 中的 jar 以及目录。

##### **（2）委派模式（Delegation Mode）**

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fb7a2a88e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

当 JVM 加载一个类的时候，下层的加载器会将任务给上一层类加载器，上一层加载检查它的命名空间中是否已经加载这个类，如果已经加载，直接使用这个类。如果没有加载，继续往上委托直到顶部。检查之后，按照相反的顺序进行加载。如果 Bootstrap 加载器不到这个类，则往下委托，直到找到这个类。一个类可以被不同的类加载器加载。  

可见性限制：下层的加载器能够看到上层加载器中的类，反之则不行，**委派只能从下到上**。

不允许卸载类：类加载器可以加载一个类，但不能够卸载一个类。但是类加载器可以被创建或者删除。

##### （3）JVM 执行引擎

类加载器将字节码载入内存后，执行引擎以 java 字节码为单元，读取 java 字节码。java 字节码机器读不懂，必须将字节码转化为平台相关的机器码。这个过程就是由执行引擎完成的。

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fb122deee?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在执行方法时 JVM 提供了四种指令来执行  

①invokestatic: 调用类的 static 方法。

②invokevirtual: 调用对象实例的方法。

③invokeinterface：将属性定义为接口来进行调用。

④invokespecial：JVM 对于初始化对象（Java 构造器的方法为：）以及调用对象实例的私有方法时。

主要的执行计数：

解释，即时执行，自适应优化、芯片级直接执行。

解释属于第一代 JVM

即时编译 JIT 属于第二代 JVM

**自适应优化**（目前 sun 的 HotspotJVM 采用这种技术），吸取第一代 JVM 和第二代 JVM 的经验，采用两者结合的方式，开始对所有的代码都采用解释执行的方式，并监视代码执行情况，然后对那些经常调用的方法启动一个后台线程，将其编译为本地代码，并进行优化。若方法不再频繁使用，则取消编译过代码，仍对其进行解释执行。

### 3、java 运行时数据区

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fd3fe6d9c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### **4、PC 寄存器**  

用于存储每个线程下一步将要执行的 JVM 指令，若该方法为 native 的，则 PC 寄存器中不存储任何信息。Java 多线程情况下，每个线程都有一个自己的 PC，以便完成不同线程上下文环境的切换。

### **5、JVM 栈**

JVM 栈是线程私有的，每个线程创建的同时都会创建 JVM 栈，JVM 栈中存放当前线程中局部基本类型的变量（Java 中定义的八种基本类型：boolean、char、byte、short、int、long、float、double）、部分的返回结果以及 Stack Frame，非基本类型的对象在 JVM 栈上仅存放一个指向堆的地址。

### **6、堆（Heap）**

它是 JVM 用来存储对象实例以及数组值的区域，可以认为 Java 中所有通过 new 创建的对象的内存都在此分配，Heap 中的对象的内存需要等待 GC 进行回收。

堆在 JVM 启动的时候就被创建，堆中储存了各种对象，这些对象被自动管理内存系统（Automatic Storage Management System），也就是常说的 “Garbage Collector（垃圾回收器）” 管理。这些对象无需、也无法显示地被销毁。

JVM 将 Heap 分为两块：新生代 New Generation 和旧生代 Old Generation

![](https://user-gold-cdn.xitu.io/2019/7/5/16bc103fdf987416?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

堆是 JVM 中所有线程共享的，因此在其上进行对象内存的分配均需要进行加锁，导致 new 对象的开销比较大。  

Sun Hotspot JVM 为了提升对象内存分配的效率，对于所有创建的线程都会分配一块独立的空间 TLAB（Thread Local Allocation Buffer），其大小由 JVM 根据运行的情况计算而得，在 TLAB 上分配对象时不需要加锁，因此 JVM 在给线程对象分配内存时会尽量的在 TLAB 上分配，在这种情况下 JVM 中分配对象内存的性能和 C 基本是一样的，但如果对象过大的话则仍然要直接使用堆空间分配。

TLAB 仅作用于新生代的 Eden Space，因此在编写 Java 程序时，通常多个小的对象比大的对象分配起来更加高效。

所有新创建的 Object 都将会存储在新生代 Young Generation 中。如果 Young Generation 的数据在一次或多次 GC 后存活下来，那么将被转移到 OldGeneration。新的 Object 总是创建在 Eden Space。

### **7、方法区域（Method Area）**

在 Sun JDK 中这块区域对应的为 PermanetGeneration，又称为持久代。

方法区域存放所加载类的信息（名称、修饰符等）、类中的静态变量、类中定义为 final 类型的常量、类中的 Field 信息、类中的方法信息，当开发人员在程序中通过 Class 对象中的 getName，isInstance 等方法来获取信息时，这些数据都来源于方法区域，同时方法区域也是**全局共享**的，在一定条件下它也会被 GC，当方法区域需要使用的内存超过其允许的大小时，就会抛出 OutOfMemory 的错误信息。

### **8、运行时常量池（Runtime Constant Pool）**

存放的为类中的固定常量信息、方法和 Field 的引用信息等，其空间从方法区域中分配。

### **9、本地方法堆栈（Native Method Stacks）**

JVM 采用本地方法堆来支持 native 方法的执行，此区域用于存储每个 native 方法调用的状态。

### 10、JVM 垃圾回收

**GC 的基本原理**：将内存中不再被使用的对象进行回收，GC 中用于回收的方法称为收集器，由于 GC 需要消耗一些资源和时间，Java 在对对象生命周期特征进行分析后，按照新生代、旧生代的方式来对对象进行收集，以尽可能的缩短 GC 对应用造成的暂停。

对新生代的对象收集称为 minor GC

对旧生代的对象收集称为 Full GC

程序中主动调用 System.gc（）强制执行的 GC 为 Full GC。

**不同的对象引用类型，GC 会采用不同的方法进行回收，JVM 对象的引用分为了四种类型**：

①强引用：默认情况下，对象采用的均为强引用（这个对象的实例没有其他对象引用时， GC 时才会被回收）

②软引用：软引用是 Java 中提供的一种比较适合于缓存场景的应用（只有内存不够的情况下才会被 GC）

③弱引用：在 GC 时一定会被 GC 回收。

④虚引用：虚引用只是用来得知对象是否被 GC。