**2 monkey 之子 monkeyrunner 使用详解**


**monkeyrunner 概述**  
    monkey虽然很强大, 但它毕竟是一款为稳定性测试准备的小工具-很难支持插件编写,也不提供截屏功能,对数据流控制的能力很微弱,更无法完成像录制和回放这样的功能(事实上,通过monkey完成像录制和回放的脚本和小工具很多,这里是针对它自身情况而言).
    既然monkey做不到,那我们就将希望寄托在它"儿子"身上--monkeyrunner应运而生.
    下面就让我们看看长江后浪推前浪,青出于蓝而胜于蓝的测试利器monkeyrunner吧.

**monkeyrunner API 详解** 
    sourcecode/prebuilts/gradle-plugin/com/android/tools/monkeyrunner/25.3.3/monkeyrunner-25.3.3.jar.  
    MonkeyRunner
    MonkeyDevice
    MonkeyImage
    


    