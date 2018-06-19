## Technical Q&A QA1747 - Debugging Deployed iOS Apps

Q：如何在没有 Xcode 调试器（debugger）的情况下调试已部署的应用程序？
A：一旦部署了应用程序，无论是通过 App Store 还是 Ad Hoc 或 Enterprise 构建，您都无法将 Xcode 的调试器附加到它。要调试问题，您需要分析设备的崩溃日志和控制台输出。

Apple Watch 崩溃日志将在配对设备上提供，也可以使用下述方法获取。

有关编写丰富的 `NSLog` 语句的更多信息，请参阅 [Improved logging in Objective-C](http://developer.apple.com/library/ios/#qa/qa1669/_index.html)。

### 1.1 获取崩溃日志和控制台输出

#### 1.1.1 从没有 Xcode 的情况下，直接从设备获取崩溃日志 - Getting Crash Logs and Console Output

您的用户可以从他们的设备检索崩溃报告，并按照以下说明通过电子邮件发送给您。无法直接从设备获取设备控制台日志。

1. 打开设置应用程序
2. 转到隐私，然后诊断和使用
3. 选择诊断和使用数据
4. 找到崩溃应用程序的日志。日志将使用以下格式命名：`<AppName>_<DateTime>_<DeviceName>`
5. 选择所需的日志。然后，使用文本选择 UI 选择日志的全部文本。一旦文本选择后，点击复制
6. 将复制的文本粘贴到邮件并根据需要发送到电子邮件地址

#### 1.1.2 使用 Xcode 从设备获取崩溃日志和控制台输出 - Getting Crash Logs and Console Output From a Device Using Xcode

即使您无法在 Xcode 的调试器中运行应用程序，Xcode 仍然可以为您提供调试问题所需的所有信息。

**使用 Xcode 6**

1. 插入设备并打开 Xcode
2. 从菜单栏中选择 Window -> Devices
3. 在左栏的 DEVICES 部分下，选择设备
4. 要查看设备控制台，请单击右侧面板左下方的向上三角形
5. 单击右下角的向下箭头将控制台另存为文件
6. 要查看崩溃日志，请选择右侧面板上“设备信息”部分下的“查看设备日志”按钮
7. 在“进程”列中找到您的应用程序，然后选择“崩溃日志”以查看内容。
8. 要保存崩溃日志，请右键单击左列的条目并选择“导出日志”
9. Xcode 6 也会在这里列出低内存日志。这些将显示进程名称“未知”和类型“未知”。您应该检查这些日志的内容，以确定这些日志中是否有任何内容是由您的应用程序引起的。有关低内存日志的更多信息，请参阅了解和分析iOS应用程序崩溃报告。

**使用 Xcode 5**

1. 插入设备并打开 Xcode
2. 打开 Organizer 窗口并选择 Devices 选项卡
3. 在左列的 DEVICES 部分下，展开设备列表
4. 选择设备日志以查看崩溃日志或选择控制台以查看控制台输出

### 1.2 启用 App Store 诊断报告 - Enabling App Store Diagnostic Reporting

崩溃日志会自动从已选择向 Apple 发送诊断和使用信息的客户收集。

从 Xcode 6.3 开始，至少运行 iOS 8.3 和 TestFlight 测试版测试器的 App Store 客户的崩溃日志可以在 Xcode Organizer 中找到。要获取这些崩溃日志：

1. 打开 Xcode 6.3 及以上版本的 Organizer 窗口
2. 选择顶部的“崩溃”。然后可以在此窗口中找到可用的崩溃日志。

[App Distribution Guide](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AnalyzingCrashReports/AnalyzingCrashReports.html) 包含有关崩溃报告服务的更多信息。

来自运行较旧 iOS 版本客户的崩溃报告可以在 iTunes Connect 中找到。

如果有人报告崩溃，并且您在 iTunes Connect 中看不到相应的报告，则应该引导他们阅读以下 [for Mac](http://support.apple.com/kb/PH19602) 或者 [for Windows](http://support.apple.com/kb/PH12477) 的文章，以便他们可以选择向您发送崩溃报告。

### 1.3 了解崩溃日志和控制台输出 - Understanding Crash Logs and Console Output
理解崩溃日志的第一步也是最重要的步骤就是对它们进行符号化。符号采用人类可读的函数名称和行号替换内存地址。

如果您通过 Xcode 的设备窗口从设备上获取崩溃日志，那么几秒钟后它们将自动为您符号化。否则，您需要自己将 `.crash` 文件导入 Xcode。打开 Xcode 设备窗口，选择相关设备，将崩溃文件拖到左侧栏，按住 Control 键并点击刚添加的文件，然后从菜单中选择 "Re-Symbolicate Log"。

有关解释崩溃日志的更多信息，请参阅 [Understanding and Analyzing iOS Application Crash Reports](http://developer.apple.com/library/ios/#technotes/tn2151/_index.html) 和 [Understanding Crash Reports on iPhone OS WWDC 2010 Session](https://developer.apple.com/videos/wwdc/2010/?id=317)。