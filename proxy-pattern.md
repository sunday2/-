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

