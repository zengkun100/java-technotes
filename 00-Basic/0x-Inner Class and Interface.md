在阅读框架的源代码的时候，经常会看到这种嵌套类的写法。下面是从Spring框架里摘抄出的一段代码（已做了大量的删减）

```java
package org.apache.commons.logging;

final class LogAdapter {

	private LogAdapter() {
	}

	private enum LogApi {LOG4J, SLF4J_LAL, SLF4J, JUL}


	private static class Log4jAdapter {

		public static Log createLog(String name) {
			return new Log4jLog(name);
		}
	}

	private static class JavaUtilAdapter {

		public static Log createLog(String name) {
			return new JavaUtilLog(name);
		}
	}

}
```

嵌套类型的使用，用官方的说法是：**可以提升代码的可读性和封装性。**

定义在一个outer class里的嵌套类，本质上也是outerclass的一个成员（member），跟outer class的变量或者方法是同等级别的事物。所以可以在定义嵌套类的时候，加上public，private等可见性修饰符。比如在上面的源代码中，都是用的private修饰符。

嵌套类最特别的一点，在于outer class和inner class的私有部分，互相对对方敞开。builder模式正是利用了这一特性：往往builder类会定义成它要build出来的产品（product）的一个嵌套类，同时将产品类的构造函数做成私有的，这样就只能通过builder访问的到。大约就是下面这个结构：

```java
public class product {
    
    private product() {

    }

    public static class builder {
        
        public product build() {
            return new product();
        }
    }
}
```

同理，既然嵌套类是outer class的一个成员（member），在定义它的时候也可以跟变量或者方法一样，加上static修饰符。在英文的文档中，non-static类型的嵌套类被称为inner class，而static的嵌套类被称为nested class。中文世界里，好像没看到这么严格的区分。有没有static修饰符，对嵌套类的使用影响比较大。

就像static类型的变量或者方法一样，static类型的嵌套类，它是属于outer class的，而不是属于某个outer class的对象的。所以实例化一个static类型的嵌套类的动作很直接：

```java
OuterClass.InnerClass i = new OuterClass.InnerClass();
```

但是non-static类型的静态类，它隶属于outer class的某个实例，为了实例化这种类型的嵌套类，必须先构造一个outer class的实例，然后再通过这个实例来创建inner class的实例：

```java
OuterClass o = new OuterClass();
OuterClass.InnerClass i = o.new OuterClass.InnerClass();
```

如果要说有什么经验之谈，那就是：**除非有什么特殊的必要性，否则嵌套类最好默认定义成static的。**

介绍完了嵌套类，嵌套接口就没啥可说的了。定义在一个接口或者一个类内部的都叫做嵌套接口。而且嵌套接口默认是public static类型的。通常嵌套接口更适合用来组合namespace：

```java
public interface OuterInterface {

    // 不需要额外注明 public static，因为默认就是
    innterface InnerInterface {

    }
}
```
