/system/core/adb/commandline.cpp
/system/core/fastboot/fastboot.cpp

/system/core/init/init.cpp


什么是原子操作? 所谓原子操作, 就是该操作绝不会在执行完毕前被任何其他任务或事件打断, 也就说,原子操作是最小的执行单位.
从内存中取数据到寄存器
对寄存器中的数据进行递增操作,结果还在寄存器中
寄存器的结果写回内存
system/core/libcutils/include/cutils/atomic.h中定义的原子操作函数:
android_atomic_inc 原子加1/自增
android_atomic_dec 原子减1/自减
android_atomic_add 原子加法
android_atomic_and 原子与
android_atomic_or 原子或
android_atomic_acquire_store 原子赋值
android_atomic_release_store 原子赋值
android_atomic_acquire_load 原子读取
android_atomic_release_load 原子读取 注意:上面这两个函数从功能上看是一样的，区别只是内存屏障位于读取前还是读取后.
android_atomic_acquire_cas 原子比较并交换
android_atomic_release_cas 原子比较并交换 注意:上面这两个函数从功能上看是一样的，区别只是内存屏障位于读取前还是读取后.
android_compiler_barrier
#define a b 写代码时用a代替b
#define android_atomic_write android_atomic_release_store 
#define android_atomic_cmpxchg android_atomic_release_cas