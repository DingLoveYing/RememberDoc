## Android学习计划

ps：每日一个知识点，要求不高，但是要把基本内容了解清楚。坚持，加油！！！！！

### Android 启动和管理方面
#### 1. Android的启动过程
  1. Android手机从按下电源键开始启动BootLoader,然后把Linux内核拷贝到内存空间，启动android的第一个用户进程init进程，并等待执行。
  2. init进程中启动了Zygote进程服务，进入ZygoteInit.java 的main方法，然后由ZygeteServe去 registerZygoteSocket，创建Socket服务
     端对象sServerSocket  --> preload方法预加载类，资源等  --> forkSystemServer（由Zygote调nativeForkSystemServer去fork子线程）
     -->  通过forkSystemServer方法返回的值，进入两个分支处理：父进程返回子进程pid值，进入到ZygoteInit类中的main方法继续处理；而子进程调用handleSystemServerProcess方法，最终会运行system_server。启动系统服务system_server  -->  runSelectLoopMode监听
     和处理sServerSocket的Socket请求.
  3. 小结
  
```
1. 系统启动时init进程会创建Zygote进程，Zygote进程负责后续Android应用程序框架层的其它进程的创建和启动工作。（可以使用ps查看）
2. Zygote进程会首先创建一个SystemServer进程，SystemServer进程负责启动系统的关键服务，如包管理服务PackageManagerService和应用程序组件管理服务ActivityManagerService。
3. 当我们需要启动一个Android应用程序时，ActivityManagerService会通过Socket进程间通信机制，通知Zygote进程为这个应用程序创建一个新的进程。
```

#### 2. Manager 与 service
* ActivityManager、ActivityManagerService、ActivityThread
* WindowManager、WindowManagerService
* PackageManager、PackageManagerService
