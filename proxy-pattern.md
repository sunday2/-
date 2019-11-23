#### 招式特点

```
代理模式是structural pattern(结构型设计模式)的一种,分为静态代理和动态代理.
(1)注意和装饰者模式的区别,装饰者模式关注的是为对象动态加上行为,而代理模式关注的是对对象的控制访问.
(2)代理模式分为很多种类,分别为远程代理,虚拟代理,保护代理,缓存代理
缺点:
(1)类的数目的增加
(2)可能会造成处理速度变慢
```

#### 招式要点

```
静态代理比较简单.接口,实现类,代理类(同样实现接口)。比较常见的应用就是日志打印。	
动态代理比较复杂,常见的应用就是aop.aop的实现就是基于动态代理的。注意和decorator pattern 的区别，proxy pattern 和被代理类是关联关系中的组合关系，而decorator pattern 和被装饰类是一种聚合关系。耦合度不一样。
```

#### 招式演进

* STEP 1

```
新建一个接口类.
```

```java
//Image.class
public interface Image {
   void display();
}
```

* STEP 2

```
具体的类(concrete class)实现接口.
```

```java
//RealImage.class
public class RealImage implements Image {

   private String fileName;

   public RealImage(String fileName){
      this.fileName = fileName;
      loadFromDisk(fileName);
   }

   @Override
   public void display() {
      System.out.println("Displaying " + fileName);
   }

   private void loadFromDisk(String fileName){
      System.out.println("Loading " + fileName);
   }
}
```



* STEP 3

```
代理类,和被代理类实现同个接口.
```

```java
//ProxyImage.class
public class ProxyImage implements Image{

   private RealImage realImage;
   private String fileName;

   public ProxyImage(String fileName){
      this.fileName = fileName;
   }

   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}
```

* STEP 4

```
使用例子.
```

```java
public class ProxyPatternDemo {
	
   public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");

      //image will be loaded from disk
      image.display(); 
      System.out.println("");
      
      //image will not be loaded from disk
      image.display(); 	
   }
}
```

* jdk实现的动态代理

```
    动态代理和静态代理体现在一个是动态一个是静态的，动态和静态实际指的是代理对象是动态生成的。在jdk的reflect包下的Proxy类就是对动态代理的实现，而另一种动态代理技术的实现是cglib，这里我们讲的是jdk动态代理的实现。
```

* jdk动态代理实现demo

```java
package proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class JdkProxyDemo {

    interface If {
        void originalMethod(String s);
    }

    static class Original implements If {
        public void originalMethod(String s) {
            System.out.println(s);
        }
    }

    static class Handler implements InvocationHandler {
        private final If original;

        public Handler(If original) {
            this.original = original;
        }

        public Object invoke(Object proxy, Method method, Object[] args)
                throws IllegalAccessException, IllegalArgumentException,
                InvocationTargetException {
            System.out.println("BEFORE");
            method.invoke(original, args);
            System.out.println("AFTER");
            return null;
        }
    }

    public static void main(String[] args){
        Original original = new Original();
        Handler handler = new Handler(original);
        If f = (If) Proxy.newProxyInstance(If.class.getClassLoader(),
                new Class[] { If.class },
                handler);
        f.originalMethod("Hallo");
    }

}
```

* 分析

```
这里为了方便，我们的例子是写在同一个类中的。分解后的步骤如下:
(1)业务类的接口类If。
(2)接口If的实现类Original，实际上这个就是要被代理类。
(3)如果要使用jdk中的动态代理Proxy，那么一定要实现reflect包下的InvocationHandler接口,重写构造函数，并实现invoke接口，该接口看起来类似代理类，因为其拥有被代理类的实例。
(4)通过Proxy的静态方法newProxyInstance动态生成被代理类的代理类。

这里，觉得要注意的是传递给newProxyInstance方法的是InvocationHandler的实现类，而传递给InvocationHandler构造函数的是被代理类。


由于jdk的动态代理的代理对象是基于接口的，它会生成目标对象的接口的子对象，所以更具松耦合性；
而cglib运行速度更快，但是生成速度偏慢。
spring aop的实现核心技术是动态代理，其默认会根据被代理对象是否有实现接口来判断是使用jdk的动态代理还是cglib，当然也可以通过配置强制使用，但是约定优于配置。
```

