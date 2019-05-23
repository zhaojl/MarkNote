**深入理解Android自动化测试（许奔）第二章 稳定性测试利器-monkey使用详解**


要想发布新版本, 得先通过稳定性测试, 要想通过稳定性测试, 得先通过monkey.

monkey可以运行在模拟器或实际设备中, 它向系统发送伪随机的用户事件流,如点击输入,触摸屏输入,手势输入等, 实现对正在开发的应用程序进行压力测试, 是一种为了测试软件的稳定性和健壮性的快速有效方式.



**monkey介绍**  
    monkey的源码在sourcecode/development/cmds/monkey/src目录下.
    monkey的基本使用  
        首先, 配置 android sdk环境, android/sdk/platforms, android/sdk/platform-tools.  
        其次, 通过命令行直接运行monkey命令.
        运行monkey 1:  
        $ adb shell /system/bin/monkey  
        运行monkey 2:  
        $ adb shell  
        s2:/ $ su  
        s2:/ # cd system/bin  
        s2:/system/bin # monkey     运行monkey脚本  
        s2:/system/bin # cat monkey  
        查看monkey脚本文件中的内容如下:  
            # Script to start "monkey" on the device, which has a very rudimentary shell.  
            base=/system  
            export CLASSPATH=$base/framework/monkey.jar  
            trap "" HUP  
            exec app_process $base/bin com.android.commands.monkey.Monkey $*  
        注:  
          /system/framework/monkey.jar和其对应的二进制文件/system/bin/monkey  
          通过脚本我们发现, 该批处理调用的是com.android.commands.monkey.Monkey, 如果要定制monkey工具, 则需要修改这个包中的文件.  
        $ adb shell monkey [options] <event-count>  
        $ adb shell monkey -p your.package.name -v 500  
        $ adb shell monkey -c Intent.CATEGORY_LAUNCHER -v 500  
        $ adb shell monkey -f /mnt/sdcard/test 1 (这里的1为循环次数而非事件数)  
        $ adb shell monkey -v -v -v -p your.package.name --throttle 5000 1000  
        $ adb shell monkey -help  
            常规类General:  
                -help  
                -v  
            约束类Constraints:  
                -p <allowed-package-name>  -p <allowed-package-name>  
                -c <main-category>  -c <main-category>      如果不指定任何类别,monkey将选择Intent.CATEGORY_LAUNCHER 和 Intent.CATEGORY_monkey里的Activity.  
                <event-count>  
            事件类Events:  
                -s <seed>           用相同的seed值再次运行,将生成相同的事件序列: adb shell -s <seed> <event-count>  
                -v -v -v ...        打印出日志信息,每个-v将增加反馈信息的级别,-v越多日志信息越详细,目前最多支持3个  
                --throttle <milliseconds> 在每一个指令之间加上固定的间隔的时间,单位毫秒  
                --randomize-throttle  
                --pct-touch <percent>  指定触摸事件百分比, 触摸事件不单单是按键,它泛指发生在某一位置的一个down-up事件.  
                --pct-motion <percent>  指定动作事件百分比, 动作事件也不单单指手势操作, 它泛指从某一位置按下(即down事件)后经过一系列伪随机事件后弹起(即up事件).  
                --pct-trackball <percent> 指定轨迹球事件百分比, 轨迹球事件包括一系列的随机移动,以及偶尔跟随在移动后面的点击事件.  
                --pct-syskeys <percent> 指定系统按键事件百分比, 系统按键事件通常指仅供系统使用的保留按键, 如HOME键,BACK键,拨号键(即Start Call),挂断键(即End Call)及音量键(即Volume Controls)等.  
                --pct-nav <percent> 指定基本导航事件百分比, 基本导航事件主要指来自方向输入设备的上,下,左,右事件, 即up,down,left,right事件.  
                --pct-majornav <percent> 指定主要导航事件百分比, 主要导航事件通常指引发图形界面的一些动作,如5-way键盘中间按键,返回按键,菜单按键等.  
                --pct-appswitch <percent> 指定应用启动事件百分比, 应用启动事件(即activity launches)俗称打开应用, 通过调用startActivity()方法最大限度的开启该package下的所有应用.  
                --pct-flip <percent>  
                --pct-anyevent <percent> 其他类型事件的百分比, 其他类型事件包含了除刚才所提到的事件外的所有事件,如keypress,不常用的button等.  
                --pct-pinchzoom <percent>  
                --pct-permission <percent>  
            调试类Debugging:  
                --wait-dbg  暂停执行中的monkey,直到有调试器与它连接.  
                --dbg-no-events  
                --kill-process-after-error  当monkey因为应用程序发生错误而停止时,将会通知系统停止发生错误的进程.  
                --hprof  
                --ignore-crashes    当应用程序崩溃或发生失控异常时,monkey将继续运行直到计数完成;  
                --ignore-timeouts   当应用程序发生任何超时错误(如ANR, Application No Responding)时,monkey将继续运行直到计数完成;  
                --ignore-security-exceptions    当应用程序发生任何权限错误(如启动一个需要某些权限的Activity)时,monkey将继续运行直到计数完成;  
                --monitor-native-crashes    monkey运行时native code的崩溃事件将被监视并报告, 如果此时还设置了--kill-process-after-error,那么system native code崩溃系统也将停止运行.  
                --ignore-native-crashes  
                --pkg-blacklist-file <package_blacklist_file>  
                --pkg-whitelist-file <package_whitelist_file>  
                --setup scriptfile  
                -f <scriptfile> -f scriptfile...   使用monkey运行指定的monkey脚本  
                --port port  
                --profile-wait <milliseconds>  
                --device-sleep-time <milliseconds>  
                --randomize-script  
                --script-log  
                --bugreport  
                --periodic-bugreport  
                --permission-target-system  
            注:  
                产生anr的条件如下:  
                1) 线程响应超过5s.  
                2) HandleMessage回调函数超过10s.  


**monkey常用API**  
  接下来从对monkey的api分析中一步步学习monkey脚本的编写, 并通过对另外两个实用命令getevent 和 input keyevent 的原理的简单分析, 让大家直观感受Android的keyevent魅力.  
  比如下面这个Bugben应用(git@github.com:qzoscar/HelloBugben.git), 能通过monkey做到自动自动填写,选择和提交吗?  
  monkey发送的随机事件主要有点击,输入和手势3种,下面就以这3种事件为入口学习一下monkey脚本编写吧.  
    
  让我们先来看手势,即传说中的轨迹球事件:  
      DispatchTrackball(long downTime, long eventTime, int action, float x, float y, float pressure, float size, int metaState, float xPrecision, float yPrecision, in device, int edgeFlags)  
  
  这么多参数,我们最关注的只有action, x, y这3个而已:  
      downTime  键最初被按下的事件;  
      eventTime 事件发生的时间;  
      action    参照android.view.MotionEvent类中定义, 0代表按下ACTION_DOWN, 1代表弹起ACTION_UP, 2代表移动ACTION_MOVE等, 要实现点击事件, ACTION_DOWN和ACTION_UP应成对出现;  
      x         x坐标;  
      y         y坐标;  
      pressure  当前事件的压力, 范围0-1;  
      size      触摸的近似值,范围0-1;  
      metaState 当前按下的meta键的标识;  
      xPrecision    x坐标精确值;  
      yPrecision    y坐标精确值;  
      device        事件来源, 范围0-x, 0表示不来自物理设备;  
      edgeFlags     坐标是否超出了屏幕范围;  
      DispatchTrackball(5109520, 5109520, 0, 1150, 330,0,0,0,0,0,0)  
      DispatchTrackball(5109520, 5109520, 1, 1150, 330,0,0,0,0,0,0)  
      
  接下来看输入字符串事件:  
      DispatchString(String text)  
  输入一个不加引号的字符串, 如DispatchString(abcd), 表示输入abcd字符.  
  
  接下来看点击事件:  
      DispatchPointer(long downTime, long eventTime, int action, float x, float y, float pressure, float size, int metaState, float xPrecision, float yPrecision, in device, int edgeFlags)
  类似DispatchTrackball中的参数.  
  
  接下来看启动应用事件:  
      LaunchActivity(com.android.browser, com.android.browser.BrowserActivity)  
  应用所在包名package_name和应用名class_name.  
  
  接下来看等待:  
      UserWait(long sleepTime)  
  操作需要等待的时间, 单位毫秒, 如UserWait(3000)表示等待3秒.  
  
  接下来看按下键值:  
      DispatchPress(int keycode)  
  具体的键值可参见android.view.KeyEvent, 如果你认为不够,还可以为Android添加新键值,网上有很多关于添加新键值的资料.  
  
  接下来看长按键值:  
      LongPress(int keycode)  
  类似DispatchPress, 只不过这个是长按.  
  
  接下来看发送键值:  
      DispatchKey(long downTime, long eventTime, int action, int code, int repeat, int metaState, int device, int scancode)  
      downTime  键最初被按下的事件;  
      eventTime 事件发生的时间;  
      action    参照android.view.MotionEvent类中定义, 0代表按下ACTION_DOWN, 1代表弹起ACTION_UP, 2代表移动ACTION_MOVE等, 要实现点击事件, ACTION_DOWN和ACTION_UP应成对出现;
      code      可参见android.view.KeyEvent;  
      repeat    重复次数;  
      metaState 当前按下的meta键的标识;  
      device    事件来源, 事件发生的设备id, 范围0-x, 0表示不来自物理设备;  
      scancode  上报点的信息;  
  
  接下来看开关软键盘:  
      DispatchFlip(boolean keyboardOpen)  
  传入布尔值表示是否打开软键盘, true表示打开, false表示关闭.  


**monkey脚本编写**  
    了解了monkey常用API, 让我们来熟悉一下monkey脚本的编写规范.  
    monkey script是按照一定的语法规则编写有序的用户事件流,并适用于monkey命令工具的脚本.  
    下面尝试写一个简单的脚本,实现如下操作:  
      (1) 打开Bugben应用;  
      (2) 在文本框1中输入数字111, 在文本框2中输入数字aaa;  
      (3) 点击提交;  
    从MonkeySourceScript类中寻找到如下使用monkey script编写规范:  
        #Start Script  
        type= user  
        count= 10  
        speed= 1.0  
        start data >>  
        #查看应用包名:adb shell ls /data/data  "com.xuben.hellobugben"  
        #查看应用主界面名:adb shell logcat|busybox grep START,启动应用,查看"cmp=com.xuben.hellobugben/.ChangeActivity"就是我们所需的主界面名.  
        #启动Bugben  
        LaunchActivity(com.xuben.hellobugben, com.xuben.hellobugben.ChangeActivity)  
        #等待500毫秒  
        UserWait(500)  
        #选中文本框1  
        captureDispatchPointer(10,10,0,230,140,1,1,-1,1,1,0,0)  
        captureDispatchPointer(10,10,1,230,140,1,1,-1,1,1,0,0)  
        #确定文本框1内容  
        captureDispatchString(111)  
        captureDispatchPress(66)  
        #选中文本框2  
        captureDispatchPointer(10,10,0,280,300,1,1,-1,1,1,0,0)  
        captureDispatchPointer(10,10,1,280,300,1,1,-1,1,1,0,0)  
        #确定文本框2内容  
        captureDispatchString(aaa)  
        captureDispatchPress(66)  
        #选择单选框1不加粗  
        captureDispatchPointer(10,10,0,446,553,1,1,-1,1,1,0,0)  
        captureDispatchPointer(10,10,1,446,553,1,1,-1,1,1,0,0)  
        #选择单选框2小号  
        captureDispatchPointer(10,10,0,280,637,1,1,-1,1,1,0,0)  
        captureDispatchPointer(10,10,1,280,637,1,1,-1,1,1,0,0)  
        #等待500毫秒  
        UserWait(500)  
        #点击提交  
        captureDispatchPointer(10,10,0,300,720,1,1,-1,1,1,0,0)  
        captureDispatchPointer(10,10,1,300,720,1,1,-1,1,1,0,0)  
    $ touch Input_bugben  
    $ adb shell rm /mnt/sdcard/Input_bugben  
    $ adb push Input_bugben /mnt/sdcard/Input_bugben  
    $ adb shell monkey -v -f /mnt/sdcard/Input_bugben 10  
    

**getevent/sendevent 和 input keyevent**  
    $ adb shell
    # cd system/bin
    # ls
    # getevent --help
    # sendevent --help
    # input --help
    注意:
        1) system/bin目录下的文件都是Android系统自带的本地程序,从文件夹名称bin可以看出是binary二进制的程序,里面主要是Linux系统自带的组件(命令).
        2) getevent和sendevent是Android系统自带获取设备的收发事件和模拟设备事件进行自动化测试, 而input keyevent在自动化测试中也有很大的作用，用于模拟常用按键等.

  getevent使用
    下面举例说明, 执行如下的命令, 然后按下手机的音量上键和下键:
        # getevent -t
               时间        设备名字/事件类型type/事件编码code/数据value  
        [  158045.603598] /dev/input/event4: 0001 0072 00000001
        [  158045.603598] /dev/input/event4: 0000 0000 00000000
        [  158045.809581] /dev/input/event4: 0001 0072 00000000
        [  158045.809581] /dev/input/event4: 0000 0000 00000000                                         
    上面这些事件的定义在 sourcecode/kernel/msm-4.4/include/linux/input.h中, 请看结构体:
        struct input_value {
            __u16 type;
            __u16 code;
            __s32 value;
        };
    上面显示很不好识别,可以使用-l参数,这样就可以很方便的辨别是按键事件, 按下/抬起音量下键:
        # getevent -tl
        [  158917.615033] /dev/input/event4: EV_KEY       KEY_VOLUMEDOWN       DOWN                
        [  158917.615033] /dev/input/event4: EV_SYN       SYN_REPORT           00000000            
        [  158917.788932] /dev/input/event4: EV_KEY       KEY_VOLUMEDOWN       UP                  
        [  158917.788932] /dev/input/event4: EV_SYN       SYN_REPORT           00000000
    接下来分析绝对事件, 一般为触摸屏事件, 同样的办法执行getevent -t, 然后按下触摸屏:
        # getevent -t
        [  159379.432872] /dev/input/event0: 0003 0039 00000163
        [  159379.432872] /dev/input/event0: 0001 014a 00000001
        [  159379.432872] /dev/input/event0: 0001 0145 00000001
        [  159379.432872] /dev/input/event0: 0003 0035 000002a6
        [  159379.432872] /dev/input/event0: 0003 0036 000005c7
        [  159379.432872] /dev/input/event0: 0000 0000 00000000
        [  159379.513998] /dev/input/event0: 0003 0039 ffffffff
        [  159379.513998] /dev/input/event0: 0001 014a 00000000
        [  159379.513998] /dev/input/event0: 0001 0145 00000000
        [  159379.513998] /dev/input/event0: 0000 0000 00000000
    上面显示很不好识别,可以使用-l参数,这样就可以很方便的辨别是按键事件, 按下/抬起音量下键:
        # getevent -tl
        [  159517.607577] /dev/input/event0: EV_ABS       ABS_MT_TRACKING_ID   00000164     /* Unique ID of initiated contact */        
        [  159517.607577] /dev/input/event0: EV_KEY       BTN_TOUCH            DOWN         /* Type of touching device */  
        [  159517.607577] /dev/input/event0: EV_KEY       BTN_TOOL_FINGER      DOWN         
        [  159517.607577] /dev/input/event0: EV_ABS       ABS_MT_POSITION_X    000002b0     /* Center X touch position */
        [  159517.607577] /dev/input/event0: EV_ABS       ABS_MT_POSITION_Y    000005d7     /* Center Y touch position */         
        [  159517.607577] /dev/input/event0: EV_SYN       SYN_REPORT           00000000            
        [  159517.712931] /dev/input/event0: EV_ABS       ABS_MT_TRACKING_ID   ffffffff     /* Unique ID of initiated contact */       
        [  159517.712931] /dev/input/event0: EV_KEY       BTN_TOUCH            UP           /* Type of touching device */   
        [  159517.712931] /dev/input/event0: EV_KEY       BTN_TOOL_FINGER      UP                  
        [  159517.712931] /dev/input/event0: EV_SYN       SYN_REPORT           00000000
    执行-p命令可以看到设备尺寸等信息:
        # getevent -p
        add device 6: /dev/input/event1
          name:     "input_mt_wrapper"
          events:
            ABS (0003): ABS_MT_TOUCH_MAJOR    : value 0, min 0, max 1, fuzz 0, flat 0, resolution 0
                        ABS_MT_POSITION_X     : value 0, min 0, max 1080, fuzz 0, flat 0, resolution 0
                        ABS_MT_POSITION_Y     : value 0, min 0, max 1920, fuzz 0, flat 0, resolution 0
                        ABS_MT_TRACKING_ID    : value 0, min 0, max 9, fuzz 0, flat 0, resolution 0
                        ABS_MT_PRESSURE       : value 0, min 0, max 255, fuzz 0, flat 0, resolution 0
    ABS_MT_POSITION_X和ABS_MT_POSITION_Y就是手机尺寸的大小.

  sendevent使用
    sendevent发送按下power按键:
        sendevent /dev/input/event4 0001 0074 00000001
        sendevent /dev/input/event4 0000 0000 00000000
        sendevent /dev/input/event4 0001 0074 00000000
        sendevent /dev/input/event4 0000 0000 00000000

  input keyevent使用
    input keyevent命令用法如下：
        input keyevent <keycode>
        例如拨打电话使用命令: input keyevent 5 
           BACK键使用命令: input keyevent 4
    关于keycode可以通过查看Monkey常用的键值表或者android.view.KeyEvent.
        0 -->  "KEYCODE_UNKNOWN"
        1 -->  "KEYCODE_MENU"
        2 -->  "KEYCODE_SOFT_RIGHT"
        3 -->  "KEYCODE_HOME"
        4 -->  "KEYCODE_BACK"
        5 -->  "KEYCODE_CALL" 


http://www.a-site.cn/people/u210399160606.html
adb shell input keyevent 4 (android.view.KeyEvent.java类中定义的keycode)


    