#### 招式特点

  ```
观察者模式在分类上属于行为模式,主要用于描述两个对象之间一对多的关系，当一个对象(一对多关系中的一)状态被改变的时候，依赖它的对象(一对多关系中的多)能够自动被通知.观察者在架构上又被称作发布-订阅模式(Publish/Subscirbe)
  ```

#### 招式要点

* 典型

```
Subject和Observer 各自分别引用对方.Observer 引用Subject是为了新建Observer的时候传入Subject作为参数,并调用Subject中的添加方法,在Subject里维护的Observer list 中添加Observer.
```

* java中的变形
```
  java中经常见到的命名方式并不是attach(添加)，detach(移除),observer等术语,而是register，unregister，listener等,没错,经常看到的监听器其实就是观察者模式，监听器就是观察者Observer.
```

#### 招式演进

* Step 1

```
//Subject.java

import java.util.ArrayList;
import java.util.List;

public class Subject {
	
   private List<Observer> observers = new ArrayList<Observer>();
   private int state;

   public int getState() {
      return state;
   }

   public void setState(int state) {
      this.state = state;
      notifyAllObservers();
   }

   public void attach(Observer observer){
      observers.add(observer);		
   }

   public void notifyAllObservers(){
      for (Observer observer : observers) {
         observer.update();
      }
   } 	
}
```

* step 2

```
Create Observer class.
Observer.java
```

```
public abstract class Observer {
   protected Subject subject;
   public abstract void update();
}
```

* step 3

```
Create concrete observer classes
```

```
//BinaryObserver.java
public class BinaryObserver extends Observer{

   public BinaryObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Binary String: " + Integer.toBinaryString( subject.getState() ) ); 
   }
}
```

```
//OctalObserver.java
public class OctalObserver extends Observer{

   public OctalObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
     System.out.println( "Octal String: " + Integer.toOctalString( subject.getState() ) ); 
   }
}
```

```
//HexaObserver.java
public class HexaObserver extends Observer{

   public HexaObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Hex String: " + Integer.toHexString( subject.getState() ).toUpperCase() ); 
   }
}
```

* Step 4

```
Use Subject and concrete observer objects.
```

```
//ObserverPatternDemo.java
public class ObserverPatternDemo {
   public static void main(String[] args) {
      Subject subject = new Subject();

      new HexaObserver(subject);
      new OctalObserver(subject);
      new BinaryObserver(subject);

      System.out.println("First state change: 15");	
      subject.setState(15);
      System.out.println("Second state change: 10");	
      subject.setState(10);
   }
}
```

* step 5

```
Verify the output.
```

```
First state change: 15
Hex String: F
Octal String: 17
Binary String: 1111
Second state change: 10
Hex String: A
Octal String: 12
Binary String: 1010
```

