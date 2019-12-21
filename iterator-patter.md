#### 招式特点

```
    Iterator pattern.迭代器模式。属于行为模式中的一种。根据名字这个设计模式我们应该不陌生，在jdk的容器类中遍历的时候我们经常会用到迭代器。迭代器就是应用在遍历容器的场景上，它可以隐藏底层的容器实现，把在元素之间游走的责任交给迭代器。
```



#### 招式要点

```
(1)迭代器一般都会有两个关键方法:
hasNext,next

(2)迭代器模式的本质是抽象出一个迭代器类，既不暴露集合的内部结构，又可让外部结构透明访问。

(3)一个容器可以有多种遍历方式，所以可以对应多个不同的迭代器，这样就暴露了不同的遍历方式给外部。
```



#### 招式演进

* step 1

```java
//Iterator.java
public interface Iterator{
    public boolean hasNext();
    public Object next();
}
```

```java
//Container.java
public interface Container{
    public Iterator getIterator();
}
```





* step 2

```java
//NameRepository.java    迭代器作为内部类存在
public class NameRepository implements Container {
   public String names[] = {"Robert" , "John" ,"Julie" , "Lora"};

   @Override
   public Iterator getIterator() {
      return new NameIterator();
   }

   private class NameIterator implements Iterator {

      int index;

      @Override
      public boolean hasNext() {
      
         if(index < names.length){
            return true;
         }
         return false;
      }

      @Override
      public Object next() {
      
         if(this.hasNext()){
            return names[index++];
         }
         return null;
      }		
   }
}
```



* step 3

```java
//IteratorPatternDemo.java
public class IteratorPatternDemo {
	
   public static void main(String[] args) {
      NameRepository namesRepository = new NameRepository();

      for(Iterator iter = namesRepository.getIterator(); iter.hasNext();){
         String name = (String)iter.next();
         System.out.println("Name : " + name);
      } 	
   }
}
```

output:

```
Name : Robert
Name : John
Name : Julie
Name : Lora
```



reference:

```
https://www.tutorialspoint.com/design_pattern/iterator_pattern.htm
```

