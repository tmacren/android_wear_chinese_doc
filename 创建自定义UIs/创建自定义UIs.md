Android Wear apps用户界面不同于在手机设备上构建。需要遵循Android Wear[设计规范](https://developer.android.com/design/wear/index.html)和[UI模式](https://developer.android.com/design/wear/patterns.html)，以确保应用通过针对可穿戴设备而优化的一致用户体验。

UI模式主要通过以下方式实现：
1. 卡片
2. 倒计时和确认
3. 长按消失
4. 2d pickers
5. 选择列表

Wearable UI Library是Android SDk中Google Repository的一部分，提供帮助你实现这些模式的类，并可以创建同时适配运行在圆形和方形屏幕设备上的布局。