创建Android Wear应用同样是使用Android SDK，一样可以访问GPU和传感器，但和手机应用开发还是有些不同：

1. 系统有个强制休眠时间。如果正在显示界面，但用户又没有进行操作，设备就会休眠。当再次唤醒后，会回到表盘主页而不是你之前的界面。如果你有东西需要持久化显示，可以推送通知到信息流中。
2. android wear应用相对于手机应用来说，显示尺寸和功能更小巧。可能是手机应用的子集，通常你可以先在手机上操作，然后将结果发送到手表。
3. 用户不需要直接下载应用到android wear设备。你只需要将android wear应用绑定到android手机应用中。当用户安装了手机应用，系统会自动安装android wear应用。可以出于开发目的，你依然可以安装应用到android wear设备中。
4. android wear应用可以访问标准的Android APIs。但不支持一下APIs：

	[android.webkit](https://developer.android.com/reference/android/webkit/package-summary.html)

	[android.print](https://developer.android.com/reference/android/print/package-summary.html)

	[android.app.backup](https://developer.android.com/reference/android/app/backup/package-summary.html)

	[android.appwidget](https://developer.android.com/reference/android/appwidget/package-summary.html)

	[android.hardware.usb](https://developer.android.com/reference/android/hardware/usb/package-summary.html)

通过调用[hasSystemFeature()](https://developer.android.com/reference/android/content/pm/PackageManager.html#hasSystemFeature(java.lang.String))，你可以检查android wear支持的功能。



 