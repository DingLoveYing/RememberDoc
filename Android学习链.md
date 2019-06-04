## Android
###Java基础
* Java Object类方法

* HashMap原理，Hash冲突，并发集合，线程安全集合及实现原理

* HashMap 和 HashTable 区别

* HashCode 作用，如何重载hashCode方法

* ArrayList与LinkList区别与联系

* GC机制

* Java反射机制，Java代理模式

* Java泛型

* Synchronized原理

* Volatile实现原理

* 方法锁、对象锁、类锁的意义和区别

* 线程同步的方法：Synchronized、lock、reentrantLock分析

* Java锁的种类: 公平锁、乐观锁、互斥锁、分段锁、偏向锁、自旋锁等

* ThreadLocal的原理和用法

* ThreadPool的用法和示例

* wait()和sleep()的区别

### Java高阶
* Java虚拟机，Java运行，Java GC机制（可达性分析法，引用计数法）

* Java对象的完整生命周期

* JVM内存模型

* 进程间通信，线程间通信

* JVM类加载机制

* Java引用类型

* 设计模式：除常用设计模式之外，特别的，反射机制，代理模式

* HTTP协议和HTTPS协议

* Socket协议，Socket实现长连接

* TCP和UDP协议

* HTTP协议中GET和POST的具体实现

* 序列化和反序列化

* 线程池的实现原理

* 数据库基础知识：多表查询、索引、数据库事务

###Android基础

* Application生命周期

* Android Activity生命周期

* Android Service、IntentService，Service和组件间通信

* Activity的onNewIntent

* Fragment的懒加载实现，参数传递与保存

* ContentProvider实例详解

* BroadcastReceiver使用总结

* include merge viewstub
  1. include: xml直接使用，相当于把include的xml拷贝过来
  2. merge：xml中必须作为根节点使用，加载时使用LayoutInflate.inflate方法渲染的时候， 第二个参数必须指定一个父容器，且第三个参数必须为true，也就是必须为merge下的视图指定一个父亲节点.

 -----------------
 
* Android消息机制

  [Android 消息机制详解](https://www.jianshu.com/p/3b8c2dbf1124)
  1. Handler的构造方法 （lopper, looper.queue, callback, async）;
  2. looper 由 android 主线程创建，具体实现在 activitythread 的main当中；
  3. handler sendmessage ----->  message 入队， message.taget = this handler;
  4. activitythread 创建looper 后调用 looper.loop死循环， looper 查询消息队列 messagequeue 如果有message，调用message.target.dispatchMessage方法，callback.handleMessage

* Binder机制，共享内存实现原理

  [Android Binder跨进程通信的原理](https://www.jianshu.com/p/4ee3fd07da14)
  
  AIDL的实现，demo

* Android 事件分发机制
  activity的事件分发 --> deecorView(viewgroup)的事件分发  --> viewgroup中view的事件分发

* Android 多线程的实现：Thread、HandlerThread、AsyncTask、IntentService、RxJava
  1. HandlerThread extends thread, 自己创建looper 和 handler, 多线程有序执行
  2. AsyncTask轻量级的异步任务，内部使用handler 异步任务执行前 --> 执行  --> 更新进度 --> 完成(取消)
  3. IntentService 中使用handlerthread 后台多任务有序执行
  4. rxjava [rxjava详解](http://gank.io/post/560e15be2dca930e00da1083)
* ActivityThread工作原理

  ActivityManager, ViewRootImpl, WindowManagerImpl

* Activity视图层级： 

  activity -> phonewindow -> decorView -> mContentParent -> xml
  
* Activity启动流程

```
	1. 应用通过startActivity或是startActivityForResult方法向ActivityManagerService发出启动请求。
	2. ActivityManagerService接收到启动请求后会进行必要的初始化以及状态的刷新，然后解析Activity的启动模式，为启动Activity做一系列的准备工作。
	3. 做完上述准备工作后，会去判断栈顶是否为空，如果不为空即当前有Activity显示在前台，则会先进行栈顶Activity的onPause流程退出。
	4. 栈顶Activity执行完onPause流程退出后开始启动Activity。如果Activity被启动过则直接执行onRestart -> onStart -> onResume过程直接启动Activity（热启动过程）。否则执行Activity所在应用的冷启动过程。
	5. 冷启动过程首先会通过Zygote进程fork出一个新的进程，然后根据传递的”android.app.ActivityThread”字符串，反射出该对象并执行ActivityThread的main方法进行主线程的初始化。
	6. Activity所在应用的进程和主线程完成初始化之后开始启动Activity，首先对Activity的ComponentName、ContextImpl、Activity以及Application对象进行了初始化并相互关联，然后设置Activity主题，最后执行onCreate -> onStart -> onResume方法完成Activity的启动。
	7. 上述流程都执行完毕后，会去执行栈顶Activity的onStop过程。

```
------------------

* 嵌套滑动实现原理
  
 [嵌套滑动机制的实现原理](https://www.jianshu.com/p/cb3779d36118)
 1. 子view实现NestedScrollingChild，设置setNestedScrollingEnabled = true, 使用NestedScrollingChildHelper。
 2. viewgourp实现NestedScrollingParent，使用NestedScrollingParentHelper
 3. 子view touch_down 的时候startNestedScroll, 通过NestedScrollingChildHelper将事件传递到父view。
 4. 子view 的move事件中  dispatchNestedPreScroll， 传递给父view，父view接收事件和消费事件

* RecyclerView与ListView(缓存原理，区别联系，优缺点)

  [RecyclerView 讲解](https://www.jianshu.com/p/a9f42289fd04)

* View的绘制原理，自定义View，自定义ViewGroup

* View、SurfaceView 与 TextureView

* 主线程Looper.loop为什么不会造成死循环

* ViewPager的缓存实现

* requestLayout，invalidate，postInvalidate区别与联系

 1. requestLayout : 当当前布局的宽高发生改变的时候, 此时需要重新调用父view的onMeaure和onLayout, 来给子view重新排版布局。当我们动态移动一个View的位置，或者View的大小、形状发生了变化的时候，我们可以在view中调用这个方法；
 2. invalidate : 让页面刷新, 重新调用onDraw方法,
 3. postInvalidate : 在子线程来让页面来进行刷新的方法，（非ui线程中使用，最终使用handler 调用 invalidate）


-----------------------------

* AndroidP新特性

* Android两种虚拟机

* ADB常用命令

* Asset目录与res目录的区别

* Android SQLite的使用入门

###Android开发高级

```
	引子：Android高级工程师招聘要求：
	1. 熟悉Android SDK，熟悉Android UI，熟悉Android各种调试工具；
	2. 有	丰富的Android应用架构能力，能够独立主导并架构App；
	3. Mobile Web 开发经验；具备各种复合技能：熟悉iOS、H5、Python、.NET等多种开发语言的优先考虑；
	4. 对Android性能优化，安全，软件加固，自动化测试有深刻认识;
	5. 博客，开源项目
```
####Android技术难点
AIDL、Binder、多进程、View的绘制流程、事件分发、消息队列等。这类知识对于定位自己为高级Android工程师的人来说是必须掌握的，同时他也是能鉴别高级和初中级工程师的一块试金石，其中binder是Android系统进程间通信最重要的手段之一，现阶段app的发展离不开多进程的运用，经常会启动例如定位、推送等需要在后台开启动的进程来来保证主进程的内存运行；所以合理的使用多进程也是十分必要的；view的绘制是我们自定义控件的理论基础，只有掌握了view是如何绘制的才能个性化的自定义控件；事件分发一直是Android开发的难点之一，也是必须掌握的；关于handler机制也是android的一块难点，因为包括Asynctask、系统启动、Intentservice等底层都是通过handler来实现的，所以掌握后handler机制不仅能提高你的实战开发能力，更能让你系统的了解整个android系统运作的情况。

#####Android框架层源码掌握
Android框架层有很多东西，以下几个是高级程序员必须要掌握的：

* Android包管理机制，核心PackageManagerService

* Window管理，核心WindowManagerService

* Android Activity启动和管理，核心ActivityManagerService

* 根Activity工作流程

* Context关联类

####各种原理，经典第三方库源码系列
* 自定义LayoutManager，RecyclerView中如何自定义LayoutManager

* VLayout实现原理，即如何自定义LayoutManager

* Glide加载原理，缓存方案，LRU算法

* Retrofit的实现与原理

* OKHttp3的使用，网络请求中的Intercept

* EventBus实现原理

* ButterKnife实现原理

* RxJava实现原理

* Dagger依赖注入

* 热修复实现原理，解决方案

* 组件化原理和解决方案

####Android进程通信以及多进程开发
Android 多进程和Application关系

经典解决方案：[多进程通信解决方案：Andromeda](https://mp.weixin.qq.com/s/PZs1wss3PizqSE8U2RGXYw)

####Android动画机制    Android绘图原理
经典学习资料：[HenCoder: 给高级Android工程师的进阶手册](https://hencoder.com/?utm_source=gank&utm_medium=website&utm_campaign=rxjava)


####Android页面恢复
Android的页面恢复采用以下两个方法：

onSaveInstanceState(Bundle outState)

onRestoreInstanceState(Bundle savedInstanceState)

onSaveInstanceState: 当Activity容易被系统销毁时，会触发该方法。具体的说

用户点击Home键

用户点击Home键，切换到其他应用程序

有电话来了等附加操作

####混合开发及Android WebView应用
混合开发涉及到的知识点主要包括：

APP调用WebView加载url

掌握WebView的封装，了解所有的WebSettings配置，掌握WebViewClient、WebChromeClient

掌握WebView和Native双向通信机制，会自己封装双向通信中间件

对WebView的封装可参考：[GitHub: AgentWeb](https://github.com/Justson/AgentWeb)

对通信中间件原理理解：[GitHub：webprogress](https://github.com/xudjx/webprogress)

####Gradle，自动化构建，持续集成相关
Android系统
Android Studio编译过程
其中使用到的编译工具：
aapt、aidl、Java Compiler、dex、 zipalign

主要步骤描述：

1. 通过aapt打包res资源文件，生成R.java、resources.arsc和res文件（二进制 & 非二进制如res/raw和pic保持原样）
2. 处理.aidl文件，生成对应的Java接口文件
3. 通过Java Compiler编译R.java、Java接口文件、Java源文件，生成.class文件
4. 通过dex命令，将.class文件和第三方库中的.class文件处理生成classes.dex
5. 通过apkbuilder工具，将aapt生成的resources.arsc和res文件、assets文件和classes.dex一起打包生成apk
6. 通过Jarsigner工具，对上面的apk进行debug或release签名
7. 通过zipalign工具，将签名后的apk进行对齐处理。

####App启动加载过程

Android虚拟机 Android App运行的沙箱原则

###Android架构

```
在Android源码中最重要的三个类：ActivityManagerService／PackageManagerService／View，推荐大家周末的时候可以去阅读下这部分的源码，阅读源码能提高我们今后设计架构自己代码的能力，同时也能从底层了解整个android系统的运行原理，其他一些比如主线程的消息循环、主线程如何和AMS如何跨进程交互、SystemServer进程中的各种Service的工作方式、AsyncTask的工作原理等。这些知识也是作为一个Android高级开发工程师必须掌握的，不能整天沉溺于ui和四大组件的交互，要站在更高的角度去考虑Android的有些问题。
```

参考资料： [我对移动端架构的思考](https://mp.weixin.qq.com/s/OEzcsPZHCVkjeUCh6hMuWg)

* MVC模式

* MVP模式

* MVVM模式

* CLEAN模式

* 组件化开发

* 跨平台开发：Flutter、ReactNative（RN未来要黄，了解一下就好）

####Android优化
android优化.jpg

###移动开发外围

####服务器开发相关

* SpringBoot技术

* Restful API开发

* 网络协议理解：TCP/IP、HTTP/HTTPS、OSI七层协议

* 授权认证协议： OAuth2.0 等

* 基本的数据库技术

* 数据缓存技术：Memcached、Redis，Web缓存原理

* 消息队列技术

* 监控、日志分析技术

####前端开发相关
前端开发知识很多，框架层出不穷，本质的东西却只有以下这些。

* 核心必备：HTML、CSS、JavaScript

* 入门提高：浏览器兼容性、自定义UI和动效

* 中级技能：框架层出不穷，当前以vue.js、react.js 为核心

* 协作开发技能：包管理、模块化，工具采用 npm、webpack等

* 高级技能：框架原理源码研究

####开发调试各种工具

* 性能分析工具：Memory Monitor

* 性能追踪及方法执行分析： TraceView

* 视图分析：Hierarchy Viewer

* ApkTool- 用于反向工程Android Apk文件的工具

* Lint- Android lint工具是一个静态代码分析工具

* Dex2Jar- 使用android .dex和java .class文件的工具
