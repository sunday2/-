#### 招式特点

```
单例模式应该是每个编程入门者最早接触也是平时编程中最有机会使用到的设计模式，也是最简单的一个设计模式。但是，老师教的入门写法在实际生产环境中不一定适用。
```

#### 招式要点

```
单例模式有很多的变体和实现，懒加载模式，饿汉模式，double lock模式等，前面三种实际生产中不采用。为了保证线程安全需要加锁，而加锁对性能影响太大了。
```

* 静态内部类

```
内部类是编译器行为，实际编译后也是一个正常的类，内部类和外部类有强耦合关系，非静态内部类默认对外部类有一个引用，注意静态内部类加载的触发条件。
```

#### 招式演进

* step1

```
该方法是线程安全的，毕竟初始化的时候就创建了该单例模式。缺点是不管是否访问实例都会创建实例，如果创建该实例需要分配大量的系统资源，那么会对系统性能造成影响。
```

```java
public class EagerSingleton {

    /** private constructor to prevent others from instantiating this class */
    private EagerSingleton() {}

    /** Create an instance of the class at the time of class loading */
    private static final EagerSingleton instance = new EagerSingleton();

    /** Provide a global point of access to the instance */
    public static EagerSingleton getInstance() {
        return instance;
    }
}
```

* step2

```
这个优缺点同第一个啦
```

```java
public class EagerStaticBlockSingleton {

    private static final EagerStaticBlockSingleton instance;

    /** Don't let anyone else instantiate this class */
    private EagerStaticBlockSingleton() {}

    /** Create the one-and-only instance in a static block */
    static {
        try {
            instance = new EagerStaticBlockSingleton();
        } catch (Exception ex) {
            throw ex;
        }
    }

    /** Provide a public method to get the instance that we created */
    public static EagerStaticBlockSingleton getInstance() {
        return instance;
    }
}
```

* step3

```
懒加载模式，为了线程安全，必须加锁，但是加锁会影响性能。
```

```java
public class LazySingleton {

    private static LazySingleton instance;

    /** Don't let anyone else instantiate this class */
    private LazySingleton() {}

    /** Lazily create the instance when it is accessed for the first time */
    public static synchronized LazySingleton getInstance() {
        if(instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

* step4
```
懒加载的double checked lock模式，同样影响性能
```

```java
public class LazyDoubleCheckedLockingSingleton {

    private static volatile LazyDoubleCheckedLockingSingleton instance;

    /** private constructor to prevent others from instantiating this class */
    private LazyDoubleCheckedLockingSingleton() {}

    /** Lazily initialize the singleton in a synchronized block */
    public static LazyDoubleCheckedLockingSingleton getInstance() {
        if(instance == null) {
            synchronized (LazyDoubleCheckedLockingSingleton.class) {
                // double-check
                if(instance == null) {
                    instance = new LazyDoubleCheckedLockingSingleton();
                }
            }
        }
        return instance;
    }
}
```

* Step 5
```
懒加载的静态内部类方式，线程安全的，又不影响性能，是best practice。注意，静态内部类只有getInstance被调用的时候才会被加载，而加载前才会初始化实例。
```

```java
public class LazyInnerClassSingleton {

    /** private constructor to prevent others from instantiating this class */
    private LazyInnerClassSingleton() {}

    /** This inner class is loaded only after getInstance() is called for the first time. */
    private static class SingletonHelper {
        private static final LazyInnerClassSingleton INSTANCE = new LazyInnerClassSingleton();
    }

    public static LazyInnerClassSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

