
# 最新版apk v5.0.1反编译源码教程

<b><font size="5" color="red">apk不会造假的，google签名的，造假得破解google签名</font></b>

##第一步
首先我们直接用一个解压apk（开发过android应该知道apk其实就是个压缩文件）,解压之后拷贝出里面classes.dex文件待用。

##第二步
*下载dex2jar工具，最新版下载链接[dex2jar下载](http://sourceforge.net/projects/dex2jar/)</br>
*解压之后，打开cmd，进入解压目录，运行命令：</br>
d2j-dex2jar.bat classes.dex(上一步解压的) jarpath(反编译dex后的文件目录)</br>
example:</br>
d2j-dex2jar.bat c:\user\qting\classes.dex c:\user\qting\ </br>
*反编译之后，会得到一个classes-dex2jar.jar文件，待用。</br>

##第三步
*下载JD-GUI(反编译jar神器)，最新版下载链接[JD-GUI下载](http://www.softpedia.com/get/Programming/Debuggers-Decompilers-Dissasemblers/JD-GUI.shtml)</br>
*解压之后，双击打开，直接把上一步得到的的classes-dex2jar.jar文件直接拖入JD-GUI里面，你就可以随意查看蜻蜓的源码了。</br>

# Summary

蜻蜓FM的Android程序员难道你们的节操都碎了么？？没有节操的你们确实很文艺--普罗米修斯，宙斯，还有阿波罗，你们是神一样的团队！
史上最牛逼造假App蜻蜓FM神一般的数据造假手段，让投资人和广告主欲哭无泪，让中国整个互联网都涨姿势了。
