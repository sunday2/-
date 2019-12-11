#### 招式特点

```
   Adapter pattern是23种设计模式中结构型的一种。23种设计模式中有些模式真的很像，一开始我就觉得适配者和代理模式很像，但是其实通过应用场景就知道其实是不一样的。通过类图也可以区分下二者的分别。
   (1)适配器和代理一样相当于加了个中间层，但是目的是不一样的。代理是出于不暴露被代理的接口的目的，而适配器是由于客户端无法修改或者重构适配者的代码，比如spring的jpa，数据源部分对于spring jpa客户端来说，是第三方提供的，spring jpa例如要兼容hibernate，他们不可能去把第三方hibernate的代码给改了，只能通过适配器模式来达到兼容和集成的目的，也就是无法改变两边的接口。举个生活中的例子，电源适配器的产生就是为了适配，220v的电源作为适配者，如果我们能直接让它变成我们所需要的5V电源，那我们要适配器干嘛，就是没法直接修改(相当于直接重构代码)，才需要多一个中间层，来间接达到我们的需求，这就是适配器的真正的目的。
   (2)生活中有许多适配器的例子，比如电源转接头，usb转接头。
   (3)适配器也被称为包装器，wrapper。
```



#### 招式要点

```
适配器模式有两种类型，一种是对象适配器，一种是类适配器。个人觉得对象适配器较好，因为java没有多继承。

对象适配器和类适配器的区别:
1.适配器是通过含有适配者还是通过继承适配者来达到拥有适配者功能的目的。

不管哪种类型适配器，关键的是三种角色:适配者，适配器，目标对象。
```



#### 招式演进

* step 1

```java
//适配者接口
interface Bird 
{ 
    // birds implement Bird interface that allows 
    // them to fly and make sounds adaptee interface 
    public void fly(); 
    public void makeSound(); 
} 
```

* step 2

```java
//适配者的实现
class Sparrow implements Bird 
{ 
    // a concrete implementation of bird 
    public void fly() 
    { 
        System.out.println("Flying"); 
    } 
    public void makeSound() 
    { 
        System.out.println("Chirp Chirp"); 
    } 
} 
```

* step 3

```java
//目标接口
interface ToyDuck 
{ 
    // target interface 
    // toyducks dont fly they just make 
    // squeaking sound 
    public void squeak(); 
} 
```



* step 4

```java
//适配器，实现目标接口。这里是对象适配器，适配者通过适配器构造函数传给适配器，适配器再调用适配者方法。所以适配器模式又叫wrapper，包装器。
class BirdAdapter implements ToyDuck 
{ 
    // You need to implement the interface your 
    // client expects to use. 
    Bird bird; 
    public BirdAdapter(Bird bird) 
    { 
        // we need reference to the object we 
        // are adapting 
        this.bird = bird; 
    } 
  
    public void squeak() 
    { 
        // translate the methods appropriately 
        bird.makeSound(); 
    } 
} 
```



* step 5

```java
class Main 
{ 
    public static void main(String args[]) 
    { 
        Sparrow sparrow = new Sparrow(); 
    
  
        // Wrap a bird in a birdAdapter so that it  
        // behaves like toy duck 
        ToyDuck birdAdapter = new BirdAdapter(sparrow); 
  
        System.out.println("Sparrow..."); 
        sparrow.fly(); 
        sparrow.makeSound(); 
  
        // toy duck behaving like a bird  
        System.out.println("BirdAdapter..."); 
        birdAdapter.squeak(); 
    } 
} 
```

Output:

```
Sparrow...
Flying
Chirp Chirp

BirdAdapter...
Chirp Chirp
```







#### 实际应用

```
1.spring AOP
2.Spring JPA
3.Spring MVC
```



reference:

```
https://www.geeksforgeeks.org/adapter-pattern/
https://juejin.im/post/5ba28986f265da0abc2b6084
```

