
环境设置好之后，就可以开始搬砖了。不能免俗，从经典的Hello World开始搬第一块砖

```java

public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

很简单的结构，保存起来并命名为：HelloWorld.java，代表是一个源代码文件。一个java类里面只要有个main方法，就可以运行起来。main方法的签名格式比较固定，沿袭下来就好了。


在源文件目录下运行
```
javac HelloWorld.java
```
编译这个源文件，生成一个.class后缀名的文件，也就是Java的字节码（ByteCode）之后，就可以实现Java语言号称的：Write once, run anywhere了。


同样是在这个目录下运行
```
java HelloWorld
```

熟悉的Hello World!字符串输出在Console中，我们成功跑起来第一个Java程序！

