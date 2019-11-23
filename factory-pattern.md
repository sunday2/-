#### 招式特点

```
  由于spring的ioc，而且只是写接口的话，很少有机会实践并体会到设计模式的好处，设计模式都体现在框架的设计里了。前段时间在写一个后台的程序，需要根据不同主题写不同的处理逻辑，那么工厂模式就可以使用到了，可以把处理逻辑封装成一个类。
  （1）避免了把创建对象的逻辑暴露给使用者
  （2）通过公共接口引用创建的对象
```

####招式要点

```
尽管工厂模式有很多不同的变体和实现，比如有使用反射的等，但由于反射过多的使用会影响到性能问题，所以更喜欢的是避免使用反射但是同样达到反射的效果。简单工厂模式,工厂方法模式，抽象工厂模式.
```

* 静态代码块

```
一般在加载类的时候执行(得看类是怎么加载的)，可以被看作类的构造器,在main方法之前执行,并且只执行一次在它的生命周期中，之后常驻内存中,不会被垃圾回收器回收.
```

#### 招式演进

* Step1

```
这个是最简单的一个简单工厂模式，但是每次有新的product的时候，就需要修改工厂类，添加新的product对应的判断，显然这样很不方便，也很容易忘记添加，代码过于凌乱。
```

```java
public class ProductFactory{
	public Product createProduct(String ProductID){
		if (id==ID1)
			return new OneProduct();
		if (id==ID2) return
			return new AnotherProduct();
		... // so on for the other Ids
		
        return null; //if the id doesn't have any of the expected values
    }
    ...
}
```


* Step2

```
 通过反射来实现的话有一个主要的缺点就是使用反射极端情况下比没有使用反射有10%左右的性能下降。注意几种Class对象的生成区别
```

```java
 //工厂类
class ProductFactory
{
	private HashMap m_RegisteredProducts = new HashMap();

	public void registerProduct (String productID, Class productClass)
	{
		m_RegisteredProducts.put(productID, productClass);
	}

	public Product createProduct(String productID)
	{
		Class productClass = (Class)m_RegisteredProducts.get(productID);
		Constructor productConstructor = cClass.getDeclaredConstructor(new Class[] { String.class });
		return (Product)productConstructor.newInstance(new Object[] { });
	}
}

//通过在产品类外部注册
public static void main(String args[]){
		Factory.instance().registerProduct("ID1", OneProduct.class);
}
//通过在产品类内部注册
class OneProduct extends Product
{
	static {
		Factory.instance().registerProduct("ID1",OneProduct.class);
	}
	...
}

//必须需要保证向工厂注册类的时候该类已经被加载，否则工厂方法会返回null,所以如果采用main类进行注册的话，可以在main类加上静态代码块来保证类已经加载，代码块使用Class.forName来加载类
class Main
{
	static
	{
		try
		{
			Class.forName("OneProduct");
			Class.forName("AnotherProduct");
		}
		catch (ClassNotFoundException any)
		{
			any.printStackTrace();
		}
	}
	public static void main(String args[]) throws PhoneCallNotRegisteredException
	{
		...
	}
}

```

* Step3

```
应该是最被推荐的做法。
```



```java
  //产品的抽象类
  abstract class Product
  {
  public abstract Product createProduct();
  ...
  }
  //产品类的具体实现
  class OneProduct extends Product
  {
  ...
  static
  {
  	ProductFactory.instance().registerProduct("ID1", new OneProduct());
  }
  public OneProduct createProduct()
  {
  	return new OneProduct();
  }
  ...
  }
  //产品工厂
  class ProductFactory
  {
  public void registerProduct(String productID, Product p)    {
  	m_RegisteredProducts.put(productID, p);
  }

  public Product createProduct(String productID){
  	((Product)m_RegisteredProducts.get(productID)).createProduct();
  }
  }

```

####  例子

```
JDK1.2中添加同步的封装器,Collections.synchronizedXxx的实现是通过工厂方法实现的。这些类实现线程安全是:将它们的状态封装起来，并对每个公有方法都进行同步，使得每次只有一个线程能访问容器的状态。
```

