alibaba/ARouter总结

注解:
@Autowired 自动装配
@Interceptor 拦截器
@Route 路由

反射:
ParameterizedType 参数化类型

目录ARouter/module-java/build/generated/source/apt/debug/com/alibaba/android/arouter/routes下生成的文件:
ARouter$$Group$$module implements IRouteGroup {
ARouter$$Group$$m2 implements IRouteGroup {
ARouter$$Interceptors$$modulejava implements IInterceptorGroup {
ARouter$$Providers$$modulejava implements IProviderGroup {
ARouter$$Root$$modulejava implements IRouteRoot {



APT(Annotation Processing Tool): 注解处理器，是javac的一个工具，用来在编译期扫描和处理注解，通过注解来生成文件(通常是java文件)。


com.google.auto.service
框架简介:Java源代码生成器的集合。
框架地址:https://github.com/google/auto


javapoet
框架简介: javapoet是一个用于生成.java源文件的Java API。
javapoet是square推出的开源java代码生成框架，提供Java Api生成.java源文件。这个框架功能非常有用，我们可以很方便的使用它根据注解、数据库模式、协议格式等来对应生成代码。通过这种自动化生成代码的方式，可以让我们用更加简洁优雅的方式要替代繁琐冗杂的重复工作。
框架地址: https://github.com/square/javapoet

ASM
框架简介: ASM是一个通用的Java字节码操作和分析框架。
官网地址: https://asm.ow2.io/