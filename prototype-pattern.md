#### 招式特点

````
prototype pattern是设计者模式中的创建型中的一种。
(1)当要创建的对象需要耗费大量资源，使用原型模式克隆已有对象可以节省资源，特别是缩小时间损耗。什么对象创建才算得上耗费大量资源呢？比如，这个对象需要大量io，网络io如查询数据库来组装这个对象。
(2)为了并发安全，有时候需要保护性复制，此时可用原型模式。
````

#### 招式要点

```
浅拷贝和深拷贝区别:
    java中有基本类型和引用类型。浅拷贝中引用类型还是和被拷贝对象中的引用类型是同一个，也就是还是同一个地址空间，不会为引用类型再开辟空间，如果你修改了被拷贝对象中引用类型对象中的基本类型，拷贝对象也会跟着变。深拷贝则会为引用类型重新开辟新的地址空间。

1.clone()方法是Object中的方法，默认是浅拷贝的。
2.如果需要深拷贝需要重写clone()方法，且重写的类一定要实现Cloneable接口(尽管该类并没有什么方法),否则会抛出CloneNotSupportedException异常。原型模式应该是深度拷贝。

何为原型模式？
原型模式在于有一个原型，通过原型来clone出原型的衍生对象(所以有一个已经new的对象)。所有支持原型的一般都会有一个公共的抽象类，由于需要复写clone方法，该抽象类会implements Cloneable接口，在抽象类复写clone方法(深度复制一般通过字节流),后续继承该原型的类有需要可以再复写该方法。
```

#### 招式演进

* step 1

```java
//原型模式比较简单，所以没什么好演进，放个典型。
public abstract class Prototype implements Cloneable {
    public Prototype clone() throws CloneNotSupportedException{
        return (Prototype) super.clone();
    }
}
	
public class ConcretePrototype1 extends Prototype {
    @Override
    public Prototype clone() throws CloneNotSupportedException {
        return (ConcretePrototype1)super.clone();
    }
}

public class ConcretePrototype2 extends Prototype {
    @Override
    public Prototype clone() throws CloneNotSupportedException {
        return (ConcretePrototype2)super.clone();
    }
}
```

 ```
上面复写的clone方法都只是简单调用了super.clone(),所以只是浅拷贝。对于复杂对象显然是不满足的。
 ```



```java
//通过内存序列化来达到deep clone的例子
class Employee implements Serializable {
    private static final long serialVersionUID = 2L;
    private String name;
    private LocalDate doj;
    private List<String> skills;
    public Employee(String name, LocalDate doj, List<String> skills) {
        this.name = name;
        this.doj = doj;
        this.skills = skills;
    }
    public String getName() { return name; }
    public LocalDate getDoj() { return doj; }
    public List<String> getSkills() { return skills; }
    // Method to deep clone a object using in memory serialization
    public Employee deepClone() throws IOException, ClassNotFoundException {
        // First serializing the object and its state to memory using ByteArrayOutputStream instead of FileOutputStream.
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream out = new ObjectOutputStream(bos);
        out.writeObject(this);
        // And then deserializing it from memory using ByteArrayOutputStream instead of FileInputStream.
        // Deserialization process will create a new object with the same state as in the serialized object,
        ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
        ObjectInputStream in = new ObjectInputStream(bis);
        return (Employee) in.readObject();
    }
    @Override
    public String toString() {
        return String.format("Employee{name='%s', doj=%s, skills=%s}", name, doj, skills);
    }
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return Objects.equals(name, employee.name) &&
            Objects.equals(doj, employee.doj) &&
            Objects.equals(skills, employee.skills);
    }
    @Override
    public int hashCode() {
        return Objects.hash(name, doj, skills);
    }
```

```
通过内存序列化的对象需要implements Serializable接口。如果在原型类实现该Serializable接口，则跟Cloneable接口会冲突了。所以，Serializable需要衍生类去implements，且在衍生类去override clone方法来达到deep clone的目的。
```

