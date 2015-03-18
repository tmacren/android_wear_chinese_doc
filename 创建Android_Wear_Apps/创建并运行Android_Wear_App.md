通过Android Wear的API可以直接访问基于Android Wear平台的可穿戴设备，可以访问设备上的传感器、GPU，创建Activities和services


手机端App和Android Wear App应该是配套的应用，繁重的任务、网络请求等由手机端App来完成，然后将结果发送给可穿戴设备


## 安装Android Wear模拟器 ##

1、点击Tools > Android > AVD Manager


2、点击Create....


3、填入AVD相应属性：
AVD Name    随便填
Device    选Android Wear Round 或Android Wear Square （即方形表盘还是圆形表盘）
Target    选Android 4.4W - API Level 20
CPU/ABI    选Android Wear ARM (armeabi-v7a)
Keyboard    选Select Hardware keyboard present
Skin根据    选的Device类型来选
Snapshot     不选
Use Host GPU    要选，用来支持wear通知


4、点击OK
5、选择你刚建的模拟器并启动


6、用模拟器配对你的手机：


从Google Play上安装一个Android Wear app的应用 

通过usb连接你的手机
在命令行中输入adb命令：adb -d forward tcp:5601 tcp:5601
手机上启动Android Wear app并连接模拟器
点击Android Wear app应用界面右上角的菜单并选择Demo Cards
你选择的卡片就会作为通知显示在你的模拟器的主界面


ps：连接Android Wear设备跟这个差不多 也是要先装Android Wear app这个应用 然后连接usb


使用adb devices命令 Android Wear设备也会被列出来



## 创建一个工程 ##


创建一个工程一般包含可穿戴设备App和手机端App两个模块，在Android Studio中点击 File > New Project 


主要需要注意选择的是
1、选择Phone and Tablet 并且Minimum SDK选择API 8: Android 2.2 (Froyo)
2、选择Wear  select并且Minimum SDK选择API 20: Android 4.4 (KitKat Wear)


完成创建向导后 在新建的工程下就会自动创建mobile和wear两个模块


## 安装Android Wear App ##


在开发的时候，可以跟开发android 手机端App一样直接运行或者使用adb install命令。


当你准备发布应用的时候，你可以把Android Wear App嵌入到你的手机端App中。当用户安装手机端App的时候，已连接Android Wear设备会自动安装上Android Wear App。


但如果你使用的Debug签名则不会自动安装Android Wear App，需要使用release签名。



## 其他相关库 ##


1、Android v4 support library （或者v13 包含v4）可以让你手机App创建的notifications 支持Android Wear设备，


如果还有问题的话，可以直接使用API Level 20来开发。




2、为了在手机端App和Android wear App之间同步及发送数据，你需要使用Google Play services APIs，详见：[http://developer.android.com/google/play-services/setup.html](http://developer.android.com/google/play-services/setup.html)



3、wearable support library 不是一个官方正式的库，是一些专为Android Wear设计的自定义Layout。非常建议使用，因为他们是Android Wear UI方面的最佳示例，但随时有可能会改动。但即使库更新了，也不会导致程序崩溃。在后续的教程中会专门介绍这个库。相关API文档可以查看：[http://developer.android.com/shareables/training/wearable-support-docs.zip](http://developer.android.com/shareables/training/wearable-support-docs.zip)
