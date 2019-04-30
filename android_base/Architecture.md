Android系统架构


参看链接:
http://gityuan.com/android/



Google官方提供的经典分层架构图中分成5层架构，从下往上依次分为:
System Apps
Java API Framework
Native C/C++ Libraries and Android Runtime
Hardware Abstraction Layer(HAL)
Linux Kernel
请看android-stack.png和android_stack_cn.jpg




JNI与NDK概念及区别
参看链接:
https://blog.csdn.net/Hi_Red_Beetle/article/details/78994767


JNI英文定义: (Java Native Interface) is a programming interface through which Java byte code can interact with any native code.
JNI中文定义：Java本地接口.
JNI作用: 用来实现Java代码与本地其他类型语言代码(主要是C、C++)间的双向交互, 即在Java代码里调用C/C++等语言的代码 或 C/C++代码调用Java代码.
特别注意：
    JNI是 Java 调用 Native 语言的一种特性
    JNI 是属于 Java 的，与 Android 无直接关系
类比http协议，http作为超文本传输协议，它规范了我们上网时从客户端到服务器端等一系列的运作流程。正因为如此，我们才能畅通无阻的上网。
JNI也一样，只不过JNI这个协议是用来沟通java代码和外部的本地代码(c/c++)。
也就是说有了JNI这个协议，我们才能够随意的让java代码调用C/C++的代码，同样C/C++的代码也可以调用java的代码。
jni.png展示了JNI协议在系统架构中处于什么位置：
在jni.png中，上层绿色的部分一般都是用Java代码写的，下层橘黄色的部分一般都是用C/C++代码写的。
可以看出，正式由于有了中间JNI的存在我们才可以在Application层通过JNI调用下层中的一些东西。


NDK: 在Android中要进行native开发，也要用到它对应的工具包，即NDK。就像Java开发要用到JDK，Android开发要用到SDK.
NDK英文定义: The Native Development Kit (NDK) is a set of tools that allows you to use C and C++ code with Android,
and provides platform libraries you can use to manage native activities and access physical device components, such as sensors and touch input.
The NDK can be useful for cases in which you need to do one or more of the following:
    1. Squeeze extra performance out of a device to achieve low latency or run computationally intensive applications, such as games or physics simulations.
    2. Reuse your own or other developers' C or C++ libraries.
NDK中文定义：NDK是Google开发的一套开发和编译工具集，可以生成动态链接库，主要用于Android的JNI开发.
NDK的作用有很多，我们简单的列举两个，比如：
        1.首先NDK可以帮助开发者“快速”开发C(或C++)的动态库。
        2.其次，NDK集成了“交叉编译器”。使用NDK，我们可以将要求高性能的应用逻辑使用C开发，从而提高应用程序的执行效率。
根据运行的设备的不同，CPU的架构也是不同，大体有如下三种常见的CUP架构：
    arm结构 ：主要在移动手持、嵌入式设备上。我们的手机几乎都是使用的这种CUP架构。
    x86结构 ： 主要在台式机、笔记本上使用。如Intel和AMD的CPU 。
    MIPS架构：多用在网关、猫、机顶盒等设备。

什么是交叉编译:
交叉编译就是在一个平台下（比如：CPU架构为X86，操作系统为Windows）编译出在另一个平台上（比如：CPU架构为arm,操作系统为Linux）可以执行的二进制代码。
Google提供的NDK就可以完成交叉编译的工作。





