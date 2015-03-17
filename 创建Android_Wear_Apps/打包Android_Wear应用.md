当发布应用的时候，需要将Android Wear应用嵌入进手机端应用，因为用户不能直接在可穿戴设备上查看或安装应用。

如果打包正确，当用户安装手机端应用后，系统会自动推送Android wear应用到配对的可穿戴设备上。


如果应用正在开发，或者是用debug签名，这个功能就没效果。在开发的时候，需要用adb install命令或者直接用相应的IDE（比如Android studio）来安装。


## 用Android Studio打包 ##



1、在手机端应用的build.gradle中声明一个Gradle dependency来指向Android Wear应用模块：

dependencies {
   compile 'com.google.android.gms:play-services:5.0.+@aar'
   compile 'com.android.support:support-v4:20.0.+''
   wearApp project(':wearable')
}


2、点击Build > Generate Signed APK...并选择你的签名文件。在你的项目根目录，Android Studio会自动导出一个签名的手机端apk，里面内嵌的Android Wear App。

另外，你还可以创建一个signingConfig规则在Android wear和手机端应用模块的build.gradle文件中来进行签名。

    android {
      ...
      signingConfigs {
    release {
      keyAlias 'myAlias'
      keyPassword 'myPw'
      storeFile file('path/to/release.keystore')
      storePassword 'myPw'
    }
      }
      buildTypes {
    release {
      ...
      signingConfig signingConfigs.release
    }
      }
      ...
    }


点击Android Studio工具栏上的Gradle按钮来运行这个assembleRelease任务。




## 手动打包 ##



如果你使用的是其他编译方式或者IDE，还可以手动打包。

1、拷贝已签名的Android Wear应用apk到手机端工程的 res/raw目录下，apk重命名为wearable_app.apk

2、创建一个res/xml/wearable_app_desc.xml文件，里面包含Android Wear应用的版本和路径信息。例如：


    <wearableApp package="wearable.app.package.name">
      <versionCode>1</versionCode>
      <versionName>1.0</versionName>
      <rawPathResId>wearable_app</rawPathResId> 
    </wearableApp>



package, versionCode, 和versionName的值要和Android Wear应用的AndroidManifest.xml文件中的一样。rawPathResId 就是apk的文件名（不带.apk后缀）。


3、在手机端应用的AndroidManifest.xml文件中，在<application>标签下添加一个meta-data 用于引用wearable_app_desc.xml 描述文件：


    <meta-data android:name="com.google.android.wearable.beta.app"
     android:resource="@xml/wearable_app_desc"/>



4、编译并签名手机端app。


很多编译工具会自动压缩res/raw目录下的文件，但Android Wear apk是已经被压缩过的。

编译工具如果再次压缩这个apk，那么Android Wear安装器就不能读取这个apk了，就会导致安装失败。

在手机端App上就会有PackageUpdateService日志信息，比如： "this file cannot be opened as a file descriptor; it is probably compressed."

Android Studio默认不会压缩apk，但如果你使用其他编译工具，要确保apk不会被二次压缩。