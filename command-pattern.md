#### 招式特点

```
command pattern，命令模式。命令模式在23种设计模式中属于行为模式。在所有能被模型化命令的行为都应该考虑命令模式。比如和第三方的交互通信。
```



#### 招式要点

```
(1)命令模式中的角色主要是这三个:
接受者(receiver):是实际执行命令的地方，被命令类所拥有,
调用者(invoker):被客户端调用,一般作为单例存在
命令类(command):被调用者所拥有

(2)命令模式的精髓在于对客户端和命令(接受者)之间的解耦。
像经常调用第三方接口，或者第三方通信，如果使用的是http通信，那么http请求就是一个命令，不管你需要调用多少不同的接口，尽管参数不一样，但是其实本质上是同一个指令(http)，这里其实就是进行封装和抽象，不同接口的参数不一样，那么request的概念是一样的，就可以抽象出一个request接口或者抽象类，其它请求都继承或实现该接口。每个请求也可以再进行抽象出一个公共接口。
```



#### 招式演进

* step 1

```java
//抽象命令接口，对实际命令进行封装。也就是这里的命令接口不是实际命令，它会对实际命令再包一层
public interface Order {
   void execute();
}
```



* step 2

```java
//实际命令对应的类
public class Stock {
	
   private String name = "ABC";
   private int quantity = 10;

   public void buy(){
      System.out.println("Stock [ Name: "+name+", 
         Quantity: " + quantity +" ] bought");
   }
   public void sell(){
      System.out.println("Stock [ Name: "+name+", 
         Quantity: " + quantity +" ] sold");
   }
}
```



* step 3

```java
//抽象命令接口的实现类，会把实际命令通过构造函数传进来。可以作为第三方接口封装层
public class BuyStock implements Order {
   private Stock abcStock;

   public BuyStock(Stock abcStock){
      this.abcStock = abcStock;
   }

   public void execute() {
      abcStock.buy();
   }
}
```



```java
public class SellStock implements Order {
   private Stock abcStock;

   public SellStock(Stock abcStock){
      this.abcStock = abcStock;
   }

   public void execute() {
      abcStock.sell();
   }
}
```



* step 4

```java
//invoker类

import java.util.ArrayList;
import java.util.List;

   public class Broker {
   private List<Order> orderList = new ArrayList<Order>(); 

   public void takeOrder(Order order){
      orderList.add(order);		
   }

   public void placeOrders(){
   
      for (Order order : orderList) {
         order.execute();
      }
      orderList.clear();
   }
}
```

* step 5

```java
//客户端
public class CommandPatternDemo {
   public static void main(String[] args) {
      Stock abcStock = new Stock();

      BuyStock buyStockOrder = new BuyStock(abcStock);
      SellStock sellStockOrder = new SellStock(abcStock);

      Broker broker = new Broker();
      broker.takeOrder(buyStockOrder);
      broker.takeOrder(sellStockOrder);

      broker.placeOrders();
   }
}
```

output:

```
Stock [ Name: ABC, Quantity: 10 ] bought
Stock [ Name: ABC, Quantity: 10 ] sold
```



学以致用:

```
Struts中，在模型层都要继承一个Action接口，并实现execute方法，其实这个Action就是命令类
```





