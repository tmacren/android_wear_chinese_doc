在android wear中表盘的实现是作为service打包进android wear应用的。当用户安装一个手持设备应用，这个应用包含了带表盘的android wear应用，这些表盘就会出现在手持设备的配对应用和android wear设备上的表盘选择列表中。当用户选择了一个表盘，android wear设备上就会显示该表盘，并回调它的service中的必要函数。


## 创建并配置你的工程 ##

在Android Studio中为你的表盘创建一个安卓工程：

1. 启动Android Studio
2. 创建一个新工程
3. 填写工程名并点下一步
4. 选择Phone and Tablet
5. 在Minimum SDK下面,选择API 18
6. 选择wear
7. 在Minimum SDK下面，选择API 21并点下一步
8. 选择Add No Activity，并点下一步
9. 点击完成
10. 点击View > Tool Windows > Project

Android Studio会在一个工程中创建wear和mobile两个模块


## 依赖库 ##

Wearable Support Library能提供你创建表盘的必要类。Google Play services client libraries (play-services 和 play-services-wearable)是为了利用android wear数据层API来在设备间进行通信。

Android Studio会自动添加这些到你的build.gradle文件中。

## 针对Eclipse ADT 下载 Wearable Support Library ##

如果你使用的是eclipse adt插件，下载 [Wearable Support Library](Wearable Support Library) 并讲它包含进你的工程中。

## 声明权限 ##

创建表盘要求PROVIDE_BACKGROUND 和 WAKE_LOCK 权限，在同时在android wear应用和手持设备应用的manifest文件中添加这些权限：

    <manifest ...>
    <uses-permission
    android:name="com.google.android.permission.PROVIDE_BACKGROUND" />
    <uses-permission
    android:name="android.permission.WAKE_LOCK" />
    ...
    </manifest>

> 注意：手持设备应用中必须包括android wear应用中声明的所有权限


## 实现service和回调函数 ##

android wear中的表盘需要作为service来实现。当一个表盘被激活，系统会回调他的service方法，当时间改变或者重要事件发生时（例如切换到环境模式或者接受到一条新的通知）表盘会被激活。

为了实现一个表盘，你要继承 CanvasWatchFaceService 和CanvasWatchFaceService.Engine类，然后你要在CanvasWatchFaceService.Engine类中重写回调函数。这些类被包含在Wearable Support Library中。


下面是一小段你需要实现的关键代码：

    public class AnalogWatchFaceService extends CanvasWatchFaceService {
    
    @Override
    public Engine onCreateEngine() {
    /* provide your watch face implementation */
    return new Engine();
    }
    
    /* implement service callback methods */
    private class Engine extends CanvasWatchFaceService.Engine {
    
    @Override
    public void onCreate(SurfaceHolder holder) {
    super.onCreate(holder);
    /* initialize your watch face */
    }
    
    @Override
    public void onPropertiesChanged(Bundle properties) {
    super.onPropertiesChanged(properties);
    /* get device features (burn-in, low-bit ambient) */
    }
    
    @Override
    public void onTimeTick() {
    super.onTimeTick();
    /* the time changed */
    }
    
    @Override
    public void onAmbientModeChanged(boolean inAmbientMode) {
    super.onAmbientModeChanged(inAmbientMode);
    /* the wearable switched between modes */
    }
    
    @Override
    public void onDraw(Canvas canvas, Rect bounds) {
    /* draw your watch face */
    }
    
    @Override
    public void onVisibilityChanged(boolean visible) {
    super.onVisibilityChanged(visible);
    /* the watch face became visible or invisible */
    }
    }
    }

> 注意：android SDK中有表盘的实例代码，继承了CanvasWatchFaceService类来实现模拟和数字时钟。实例代码的位置在android-sdk/samples/android-21/wearable/WatchFace目录下。

CanvasWatchFaceService类提供一个类似于View.invalidate()方法的刷新机制。如果你需要系统重绘你的表盘，你可以调用invalidate()方法来实现。你只能在主线程调用invalidate()方法。如果要在其他线程刷新画面，可以调用postInvalidate()方法。


## 注册表盘Service ##

在你实现完表盘service后，你需要在android wear应用的manifest文件中注册。当用户安装了这个应用，系统会使用关于service的信息来使表盘在android wear配对应用和表盘选择列表中生效。

下面是一小段注册表盘的代码，添加到application元素下：

    <service
    android:name=".AnalogWatchFaceService"
    android:label="@string/analog_name"
    android:allowEmbedded="true"
    android:taskAffinity=""
    android:permission="android.permission.BIND_WALLPAPER" >
    <meta-data
    android:name="android.service.wallpaper"
    android:resource="@xml/watch_face" />
    <meta-data
    android:name="com.google.android.wearable.watchface.preview"
    android:resource="@drawable/preview_analog" />
    <meta-data
    android:name="com.google.android.wearable.watchface.preview_circular"
    android:resource="@drawable/preview_analog_circular" />
    <intent-filter>
    <action android:name="android.service.wallpaper.WallpaperService" />
    <category
    android:name=
    "com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
    </service>


android wear配对应用和表盘选择列表会通过com.google.android.wearable.watchface.preview标签来识别表盘。
android wear设备在hdpi屏幕分辨率下，表盘预览图通常是320*320像素尺寸。

表盘在方形和圆形设备上的预览图本质是不同的。为了定义圆形屏幕的预览图，可以使用com.google.android.wearable.watchface.preview_circular标签。
如果一个表盘包含了所有形状的预览图，在android wear配对应用和表盘选择列表中会根据设备屏幕形状选择一个合适的。如果没有包含圆形屏幕预览图，方形屏幕预览图会同时用于方形和圆形设备。对于圆形设备，方形预览图会被裁剪成圆形。

android.service.wallpaper标签定义了watch_face.xml资源文件，这个文件中包含了wallpaper元素：

    <?xml version="1.0" encoding="UTF-8"?>
    <wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />


你的android wear应用可以包含不止一个表盘。你必须在manifest文件中为你的每个表盘实现都添加一个service。 

