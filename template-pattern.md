#### 招式特点

```
template pattern.模板模式，是23种设计模式中行为模式中的一种，也是比较容易理解的一种设计模式。
```



#### 招式要点

```
在模板方法中主要是两个角色，一个是抽象类(Abstract Class)，一个是具体子类(Concrete Class)。
抽象类中有一个final修饰的方法，这个方法就是所谓的模板方法，模板方法里定义了该方法的固定工作流。
抽象类中还有其它抽象方法，这些抽象方法是模板方法工作流的组成部分，是需要子类个性化实现的。
```



#### 招式演进

* step 1

```java
//Create an abstract class with a template method being final
public abstract class Game{
    abstract void initialize();
    abstract void startPlay();
    abstract void endPlay();
    
    //template method 
    public final void play(){
        //initialize the game
        initialize();
        startPlay();
        endPlay();
    }
}
```





* step 2

```java
//create concrete classes extending the above class.
public class Cricket extends Game{
     @Override
   void endPlay() {
      System.out.println("Cricket Game Finished!");
   }

   @Override
   void initialize() {
      System.out.println("Cricket Game Initialized! Start playing.");
   }

   @Override
   void startPlay() {
      System.out.println("Cricket Game Started. Enjoy the game!");
   }
}
```



```java
public class Football extends Game {

   @Override
   void endPlay() {
      System.out.println("Football Game Finished!");
   }

   @Override
   void initialize() {
      System.out.println("Football Game Initialized! Start playing.");
   }

   @Override
   void startPlay() {
      System.out.println("Football Game Started. Enjoy the game!");
   }
}
```



* step 3

```java
//Use the Game's template method play() to demonstrate a defined way of playing game
public class TemplatePatternDemo {
   public static void main(String[] args) {

      Game game = new Cricket();
      game.play();
      System.out.println();
      game = new Football();
      game.play();		
   }
}
```



output:

```java
Cricket Game Initialized! Start playing.
Cricket Game Started. Enjoy the game!
Cricket Game Finished!

Football Game Initialized! Start playing.
Football Game Started. Enjoy the game!
Football Game Finished!
```





#### 学以致用

```
(1)Servlet和HttpServlet中应用了模板方法。
(2)Mybatis中的Excutor中运用了模板方法。
```



reference:

```
https://juejin.im/post/5bbe2bc9f265da0a867c57d8

https://www.tutorialspoint.com/design_pattern/template_pattern.htm
```

