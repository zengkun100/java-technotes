
开发环境设置好之后，就可以开始搬砖了。不能免俗，我们从经典的Hello World开始搬第一块砖！

```java
public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

很简单的结构，保存起来并命名为：HelloWorld.java，代表是一个源代码文件。一个java类里面只要有个main方法，就可以运行起来。main方法的签名格式比较固定，沿（chao）用（xi）下来就好了。


在源文件目录下运行
```
javac HelloWorld.java
```
编译这个源文件，生成一个.class后缀名的文件，也就是Java的字节码（ByteCode）文件。靠着这个字节码文件，再加上Java的虚拟机（JVM），就可以实现Java语言号称的：Write once, run anywhere了。


同样是在这个目录下运行（注意这里指定的是类名）
```
java HelloWorld
```

熟悉的Hello World!字符串输出在Console中，我们成功跑起来第一个Java程序！


另外一种日后会经常打交道的Java文件格式叫做jar包，是java archive的缩写。光看这个名字，你就能大致猜出来这个文件的结构了吧。

在命令行之行

jar -cf HelloWorld.jar HelloWorld.class

把前一步编译出来的class文件打成jar包。单独一个class文件打成jar包意义不大，但是当你的项目中有成百上千个class文件之后，打成一个jar包之后会大大的提升程序分发的便利性。

这个时候，如果我们执行前面刚刚打好的jar包
java -jar HelloWorld.jar
并没有看到预计的Hello Wold，而是一行错误提示：
no main manifest attribute, in HelloWorld.jar

是的，我们打包的时候没有指明manifest文件，Java不知道应该去执行哪个class中定义的main文件。

我们创建一个manifest.txt文件，内容如下(注意有个空行）：

```txt
Main-Class: HelloWorld

```
然后再次打包，这次更改一下命令行参数，增加manifest文件
jar -cfm HelloWorld.jar manifest.txt HelloWorld.class

再运行一次重新打好的jar包，就能看到Hello Wold的输出了

写到这里，这一章还不能画上句号。在真实的环境中，我们会大量的使用package来规划类的层级结构。像本章开头的那种不在package中的HelloWorld类，你在真实工作中不可能遇到，它很有可能长这样：

``` java
package com.example.hello;

public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

而一旦有了这个package之后，再去执行前面的编译，打包，运行等命令的时候，就可能会出现让人困惑的错误。不信你把这个文件编译成clas文件后之后再次运行下，你会看到：
```console
zengkun:hello/ $ java HelloWorld
Error: Could not find or load main class HelloWorld
```

因为有了这个package之后，HelloWorld类的真实名字其实成了com.example.hello.HelloWorld。那是不是写成类的全名就能好了呢？
```console
zengkun:artifact/ $ java com.example.hello.HelloWorld
Error: Could not find or load main class com.example.hello.HelloWorld
```

还是错

当一个类放在了package下之后，在运行的时候会去目录结构下查找这个类。举例来说：此时的HelloWorld类，应该在com/example/hello这个目录之下。让我们退回到com这一层目录，再次运行就好了
```
zengkun:java-technotes/ $ java com.example.hello.HelloWorld
Hello World!
```

但是很显然啊，你总不能每次都在com那一层目录下去运行class文件吧。当我此时处于个人的根目录（也就是$HOME目录）下的时候，我该怎么执行com.example.hello.HelloWorld类呢？

这时就需要引入classpath这个概念了。就是告诉java该去哪个目录下去寻找class文件。假定我刚才的工作目录是/Users/zengkun/Work/java-technotes，通过cp参数在运行时指定classpath，就又能运行成功了。

```console
zengkun:~/ $ java -cp "/Users/zengkun/Work/java-technotes" com.example.artifact.HelloWorld
Hello World!
```

同样的，此时要打jar包也不应该在最底层的目录下了，得上到com这一层目录，新的打包命令行如下：
```console
zengkun:java-technotes/ $ jar -cfm HelloWorld.jar com/example/hello/manifest.txt com/example/hello/HelloWorld.class
```
对这个打好的HelloWorld.jar包，不管我把它移动到什么地方，运行java -jar HelloWorld.jar都能输出正确的结果。

前面说过jar包就是一个archive包。我们打开HelloWorld.jar包，看看它里面的内容