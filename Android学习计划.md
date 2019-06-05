## Android学习计划

ps：每日一个知识点，要求不高，但是要把基本内容了解清楚。坚持，加油！！！！！

### Android 启动和管理方面
#### 1. Android的启动过程
  1. Android手机从按下电源键开始启动BootLoader,然后把Linux内核拷贝到内存空间，启动android的第一个用户进程init进程，并等待执行。
     （通俗的说，android开机启动加载linux内核后，会有linux的init进行去加载init.rc配置文，在这个文件中会启动Zygote进程，在Andrdoid 8.0中
      区分了32位和64位）。
  2. init进程中启动了Zygote进程服务，进入ZygoteInit.java 的main方法，然后由ZygeteServe去 registerZygoteSocket，创建Socket服务
     端对象sServerSocket  --> preload方法预加载类，资源等  --> forkSystemServer（由Zygote调nativeForkSystemServer去fork子线程）
     -->  通过forkSystemServer方法返回的值，进入两个分支处理：父进程返回子进程pid值，进入到ZygoteInit类中的main方法继续处理；而子进程调用handleSystemServerProcess方法，最终会运行system_server。启动系统服务system_server  -->  runSelectLoopMode监听和处理sServerSocket的Socket请求。
     (创建JavaVM并为JavaVM注册JNI，创建服务端Socket，启动SystemServer进程。)
  3. 在SystemServer中会启动一系列的服务startBootstrapServices(); (例如：ActivityManagerService，PackageManagerService等等) --> startCoreServices();（例如 ：StartBatteryService、WebViewUpdateService等）  --> startOtherServices();（例如： StartAccountManagerService、StartInputManagerService、WindowManagerService等）。
  4. 启动一个Home应用程序Launcher。应用程序Launcher在启动过程中会PackageManagerService返回系统中已经安装的应用程序的信息，并将这些信息封装成一个快捷图标列表显示在系统屏幕上，这样用户可以通过点击这些快捷图标来启动相应的应用程序。(在SystemServer把一些服务启动完成之后，会调用mActivityManagerService.systemReady()方法，其中会调用startHomeActivityLocked() --> getHomeIntent()  --> startHomeActivity)。
  
```
1. 系统启动时init进程会创建Zygote进程，Zygote进程负责后续Android应用程序框架层的其它进程的创建和启动
工作。（可以使用ps查看）
2. Zygote进程会首先创建一个SystemServer进程，SystemServer进程负责启动系统的关键服务，如包管理服务
PackageManagerService和应用程序组件管理服务ActivityManagerService。
3. 当我们需要启动一个Android应用程序时，ActivityManagerService会通过Socket进程间通信机制，通知
Zygote进程为这个应用程序创建一个新的进程。
```
  
#### 2. 点击Launcher上的图标的启动过程  
   点击事件开始代码如下：/Volumes/android/WORKING_DIRECTORY/packages/apps/Launcher3/src/com/android/launcher3/ItemClickHandler
```
private static void onClick(View v) {
        // Make sure that rogue clicks don't get through while allapps is launching, or after the
        // view has detached (it's possible for this to happen if the view is removed mid touch).
        if (v.getWindowToken() == null) {
            return;
        }

        Launcher launcher = Launcher.getLauncher(v.getContext());
        if (!launcher.getWorkspace().isFinishedSwitchingState()) {
            return;
        }

        Object tag = v.getTag();
        if (tag instanceof ShortcutInfo) {
            onClickAppShortcut(v, (ShortcutInfo) tag, launcher);
        } else if (tag instanceof FolderInfo) {
            if (v instanceof FolderIcon) {
                onClickFolderIcon(v);
            }
        } else if (tag instanceof AppInfo) {
            startAppShortcutOrInfoActivity(v, (AppInfo) tag, launcher);
        } else if (tag instanceof LauncherAppWidgetInfo) {
            if (v instanceof PendingAppWidgetHostView) {
                onClickPendingWidget((PendingAppWidgetHostView) v, launcher);
            }
        }
    }

```
这里Launcher中条目点击回调接口是ItemClickHandler，在onClick中我们看到会调用startAppShortcutOrInfoActivity函数一路追下去最终会调用父类BaseDraggingActivity的startActivitySafely：

```
public boolean startActivitySafely(View v, Intent intent, ItemInfo item) {
        if (mIsSafeModeEnabled && !Utilities.isSystemApp(this, intent)) {
            Toast.makeText(this, R.string.safemode_shortcut_error, Toast.LENGTH_SHORT).show();
            return false;
        }

        // Only launch using the new animation if the shortcut has not opted out (this is a
        // private contract between launcher and may be ignored in the future).
        boolean useLaunchAnimation = (v != null) &&
                !intent.hasExtra(INTENT_EXTRA_IGNORE_LAUNCH_ANIMATION);
        Bundle optsBundle = useLaunchAnimation
                ? getActivityLaunchOptionsAsBundle(v)
                : null;

        UserHandle user = item == null ? null : item.user;

        // Prepare intent
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        if (v != null) {
            intent.setSourceBounds(getViewBounds(v));
        }
        try {
            boolean isShortcut = Utilities.ATLEAST_MARSHMALLOW
                    && (item instanceof ShortcutInfo)
                    && (item.itemType == Favorites.ITEM_TYPE_SHORTCUT
                    || item.itemType == Favorites.ITEM_TYPE_DEEP_SHORTCUT)
                    && !((ShortcutInfo) item).isPromise();
            if (isShortcut) {
                // Shortcuts need some special checks due to legacy reasons.
                startShortcutIntentSafely(intent, optsBundle, item);
            } else if (user == null || user.equals(Process.myUserHandle())) {
                // Could be launching some bookkeeping activity
                startActivity(intent, optsBundle);
            } else {
                LauncherAppsCompat.getInstance(this).startActivityForProfile(
                        intent.getComponent(), user, intent.getSourceBounds(), optsBundle);
            }
            getUserEventDispatcher().logAppLaunch(v, intent);
            return true;
        } catch (ActivityNotFoundException|SecurityException e) {
            Toast.makeText(this, R.string.activity_not_found, Toast.LENGTH_SHORT).show();
            Log.e(TAG, "Unable to launch. tag=" + item + " intent=" + intent, e);
        }
        return false;
    }
```
然后调用startActivity方法，该方法中会ActivityManagerProxy,调用startActivity向AMS发送请求，然后AMS处理startActivity。
[Android点击Launcher应用图标的应用程序启动过程（栈和进程的创建)](https://www.jianshu.com/p/ae7c130ea3cb)

##### Activity的启动


#### 3. Manager 与 service
* ActivityManager、ActivityManagerService、ActivityThread
* WindowManager、WindowManagerService
* PackageManager、PackageManagerService
