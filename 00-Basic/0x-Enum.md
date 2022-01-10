Enum值得专门开一章，它的结构还是有一丢丢的特殊的

定义枚举，既不是class关键字，也不是interface关键字，得用专门的关键字：enum 

因为枚举默认派生自java.lang.Enum，所以enum不能再extends其它的class，但是可以implements其它的interface

```java
public enum SomeStatusEnum implements SomeInterface {
    // enum的最开始用来定义枚举常量，这些枚举常量其实是构造了enum的一个个实例
    CREATED(1, "已创建") {
        @Override
        public String toEnglish() {
            return "created";
        }
    },  // 每一个常量和下一个常量之间，用,隔开
    RUNNING(2, "运行中") {
        @Override
        public String toEnglish() {
            return "running";
        }
    }, 
    TERMINATED(3, "已终止") {
        @Override           
        public String toEnglish() {
            return "terminated";
        }
    }
    ;   //最后用一个;作为枚举常量声明段的结束

    // 如果需要额外的field，那么必须在枚举常量结束之后
    private final Integer statusCode;

    private final String statusDesc;

    // private修饰的构造函数，用于初始化field
    private SomeStatusEnum(Integer statusCode, String statusDesc) {
        this.statusCode = statusCode;
        this.statusDesc = statusDesc;
    }

    // 实现SomeInterface接口里定义的方法
    @Override
    public String functionFromTheInterface() {
        return "Hello World!";
    }

    // 这个是enum最牛逼的地方，还可以定义抽象方法，这样每个枚举常量里都需要实现这个方法
    public abstract String toEnglish();
}
```


```java
SomeStatusEnum status = SomeStatusEnum.CREATED;
SomeStatusEnum status = SomeStatusEnum.valueOf("CREATED");
```

通过上面的方式，可以定义出一个枚举的常量。用在switch语句里的时候比较简洁

```java
switch(status) {
    case CREATED:
        xxx
        break;
    case RUNNING:
        xxx
        break;
    case TERMINATED:
        xxx
        break;
}
```

用在if语句里的时候稍显麻烦一点

```java
if (status == SomeStatusEnum.CREATED) {

} else if (status == SomeStatusEnum.RUNNING) {

} else if (status == SomeStatusEnum.TERMINATED) {

}
```
