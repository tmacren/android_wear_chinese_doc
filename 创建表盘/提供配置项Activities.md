当用户安装了包含带表盘android wear app，这些表盘就会出现在可选的表盘列表中。

一些表盘支持配置参数，让用户可以自定义表盘的样式和行为。例如一些表盘让用户选择一个自定义的背景色，并且提供两种不同的时区让用户选择。

表盘支持配置参数让用户选择，是通过Activity的方式。用户可以在某个设备或者多个设备上启动配置项的activity。

Android SDK中有数字表盘示例，展示了如何实现配置项的activity，以及如何响应配置更新。示例的位置在android-sdk/samples/android-21/wearable/WatchFace目录下。


## 为配置项的activities定义一个intent ##

如果你的表盘包含配置项的activities，填加以下metadata元素到android wear app的manifest中：

    <service
    android:name=".DigitalWatchFaceService" ... />
    <!-- companion configuration activity -->
    <meta-data
    android:name=
       "com.google.android.wearable.watchface.companionConfigurationAction"
    android:value=
       "com.example.android.wearable.watchface.CONFIG_DIGITAL" />
    <!-- wearable configuration activity -->
    <meta-data
    android:name=
       "com.google.android.wearable.watchface.wearableConfigurationAction"
    android:value=
       "com.example.android.wearable.watchface.CONFIG_DIGITAL" />
    ...
    </service>

需要在元素值前面添加你的应用包名。

如果你的表盘只包含一个配置项的activity，那么只需要在对应的app中配置。


## 创建配置项activity ##

配置项activity为表盘提供有限的可配置项集合，因为复杂的菜单在小屏幕上很难操作。

声明activity：

    <activity
    android:name=".DigitalWatchFaceWearableConfigActivity"
    android:label="@string/digital_config_name">
    <intent-filter>
    <action android:name=
    "com.example.android.wearable.watchface.CONFIG_DIGITAL" />
    <category android:name=
    "com.google.android.wearable.watchface.category.WEARABLE_CONFIGURATION" />
    <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
    </activity>

这个action的名称要和前面定义的名臣匹配。

在你的配置项activity中，构建简单的ui类提供配置项给用户选择。当用户做出选择后，用户数据层API来把配置变化传给表盘的activity。

更多细节，请查看表盘示例代码中的DigitalWatchFaceWearableConfigActivity 类和DigitalWatchFaceUtil 类。


## 创建一个配对设备的配置项activity ##

配对设备上的配置项activity给用户提供表盘的所有配置项，因为他更容易在大屏幕的手持设备上展现复杂的菜单。

为了创建一个配对设备的配置项activity，添加一个新的activity到你的手持设备app模块中，并且在该app的manifest文件里声明以下intent filter：

    <activity
    android:name=".DigitalWatchFaceCompanionConfigActivity"
    android:label="@string/app_name">
    <intent-filter>
    <action android:name=
    "com.example.android.wearable.watchface.CONFIG_DIGITAL" />
    <category android:name=
    "com.google.android.wearable.watchface.category.COMPANION_CONFIGURATION" />
    <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
    </activity>


在配置项的activity中，构建提供表盘所有可选项的UI。当用户选择了配置项，使用数据层API来跟表盘activity进行通信以改变配置。

更多详情，可查看表盘示例中的DigitalWatchFaceCompanionConfigActivity 类。


## 在Android Wear App中创建一个监听的service ##

为了接受配置参数的更新，创建一个通过数据层API实现了WearableListenerService 接口的service。当配置参数发生改变的时候，你的表盘实现了重绘。

更多细节，可查看表盘示例代码中的DigitalWatchFaceConfigListenerService 类和DigitalWatchFaceService 类。