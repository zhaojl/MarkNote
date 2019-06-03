**4 终极自动化框架 UIAutomator 使用详解**


**UIAutomator 概述**  
    Android官网主推的自动化框架UIAutomator有哪些优点:
        首先,它注入原生事件进行模拟;
        其次, 它具有很好的耦合性;
        第三, 它支持跨应用操作;
        第四, 它可方便的进行事件监控;
        第五, 它扩展性非常好;
        第六, 它可借助Instrumentation框架运行;
        最后, 它非常容易上手;
    UiAutomation框架官方简介https://blog.csdn.net/zhubaitian/article/details/40504827


**第七个Impossible Mission**  
    十万火急, 研发部门昨晚发出紧急通知: 出于安全性考虑, 将不再给测试人员提供源码.
    没有源码, Instrumentation脚本就没法维护了, 这将导致全线瘫痪....
    程序员们这是找抽的节奏吗, 几天不提bug, 他们居然敢逼宫!!! 那就让他们尝尝倚天剑的滋味吧.


**更清晰的控件捕获** 
    Instrumentation通过无敌的控件捕获工具HierarchyViewer捕获界面控件树, 通过HierarchyViewer捕获的控件里的文字是乱码.
    UIAutomator通过UIAutomatorViewer捕获界面控件树,通过HierarchyViewer捕获的控件能看到清晰的树状菜单和详细的控件信息(中文显示的不是乱码, 而且显示得得信息更丰富).
    


**更直观的测试项目创建** 
    UiAutomator提供了以下两种工具来支持UI自动化测试：
        uiautomatorviewer: 用来分析UI控件的图形界面工具, 文件所在路径~/Android/Sdk/tools/bin/uiautomatorviewer.
        uiautomator.jar: 一个java库, 提供执行自动化测试的各种API.
        源码中路径sourcecode/prebuilts/gradle-plugin/com/android/tools/uiautomatorviewer/25.3.3/uiautomatorviewer-25.3.3.jar.
    下面开始创建测试项目(git@github.com:qzoscar/HelloBugben.git),按照官网推荐的步骤如下:
    第一步, 将待测应用安装到设备上.
    第二步, 识别应用UI组件(UI components): UI组件应包含文本标签(text labels)或描述标签(android:contentDescription), 或两者都有.
          识别步骤如下:
          1) $ cd ~/Android/Sdk/tools/bin
          2) $ ./uiautomatorviewer
          3) 点击左上第二个Device ScreenShot(uiautomator dump)按钮加载当前界面;
          4) 选中查看 View 的 resource-id 属性 和 bounds属性;
    第三步, 确保该应用可访问.
        1) 对于ImageButton,ImageView和CheckBox等控件需要包含控件描述属性, android:contentDescription属性, 这对于UIAutomator自动化脚本的稳定性和移植性非常重要.
        2) 对于EditText等控件需要包含提示文本属性, android:hint属性, 编辑框的提示文本对于抓取该控件至关重要;
        3) 对于控件的图标最好关联 android:hint属性;
        4) 确保用户界面上所有元素的定向控制器(directional controller), 如轨迹球,D-pad等均正常使用;
        5) 通过UIAutomatorViewer工具确保UI组件支持测试框架, 这里指UIAutomator测试框架;
        注意: 这一步主要是确认一个应用对UIAutomator工具的支持度,此处官网中提到的几点都是实际项目中非常关键的点;
    第四步, 配置测试环境, 即引入UIAutomator测试包, 具体步骤如下:
        1) 创建测试项目;
        2) 在build.gradle文件中添加如下依赖和配置:
            android {
                ......
                defaultConfig {
                    minSdkVersion 19
                    testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
                }
            }
            dependencies {
                ......
                testImplementation 'junit:junit:4.12'
                androidTestImplementation 'com.android.support.test.uiautomator:uiautomator-v18:2.1.2'
            }
        3) 将工程切换到Android视图, 找到androidTest目录, 添加UiTest.java和ExampleInstrumentedTest.java文件;
        4) 进入UiTest, 点击类名左边的Run Test按钮开始执行测试程序;



自动化测试框架对比(UIAutomator、Appium、Robotium)
    