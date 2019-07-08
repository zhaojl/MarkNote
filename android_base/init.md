Init进程是系统的第一个用户进程,也称一号进程, 系统中的其他进程都是Init进程的后代, Init进程需要在这些"后代"死亡时负责"清理"它们, 以防止它们变成"僵尸"进程.


"僵尸"Zombie进程介绍
进程调用exit()函数退出...

使用下面命令查看进程的状态:
adb shell ps
进程状态"Z"的是"僵尸"进程

使用下面命令显示系统所有属性值:
adb shell getprop

使用下面命令查看系统的环境变量:
adb shell export -p

查看最上层成activity名字
adb shell dumpsys activity | grep "mFocusedActivity"

查看包名：
adb shell ls data/data

查看包名和app具体路径：
adb shell pm list package -f

查看应用主界面：
adb shell logcat | grep START, 启动应用,查看"cmp="后面就是我们所需的主界面名.

