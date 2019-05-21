Ubuntu:/home/zhaojl/.AndroidStudio3.1/system/log

Caused by:
Could not GET 'https://dl.google.com/dl/android/maven2/com/android/tools/build/gradle/3.2.1/gradle-3.2.1.pom'.
Connect to dl.google.com:443 [dl.google.com/220.255.2.153] failed: connect timed out

原因:
1. hosts是一个没有扩展名的系统文件，可以用记事本等工具打开，其作用就是将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，
当用户在浏览器中输入一个需要登录的网址时，系统会首先自动从hosts文件中寻找对应的IP地址，一旦找到，系统会立即打开对应网页，
如果没有找到，则系统会再将网址提交DNS域名解析服务器进行IP地址的解析。
2. 需要注意的是，hosts文件配置的映射是静态的，如果网络上的计算机更改了请及时更新IP地址，否则将不能访问。


解决步骤:
Step 1:
zhaojl@zhaojl:~$ sudo gedit /etc/hosts

Step 2:
删掉hosts文件中下列配置:
220.255.2.153   www.google.com
220.255.2.153   dl.google.com
220.255.2.153   dl-ssl.google.com

Step 3:
zhaojl@zhaojl:~$ source /etc/hosts

解决此问题时参考的链接:
https://jingyan.baidu.com/article/e75aca856d956e142fdac650.html

知识拓展的链接:
https://baike.baidu.com/item/hosts/10474546?fr=aladdin
https://www.pzboy.com/computer/google-hosts/