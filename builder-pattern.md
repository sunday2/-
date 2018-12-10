#### 招式特点

```
    建造者模式将一个复杂的对象的构建与它的表示分离，属于对象创建型模式，又称为生成器模式。一个对象的状态有两种方式进行设置一种是构造器参数，一种是对象的set方法。
    （1）直接通过构造器参数若参数太多，则需要对其进行组合，这样对象就会有许多个构造器，且参数过多容易把参数顺序搞混，可读性不强；
    （2）set方法进行对象状态设置，代码臃肿，不够优雅。
```

####招式要点

* 静态内部类
```
(1)为什么是内部类？
内部类和外部类有强关联关系，但是又和其它类无耦合，提高可读性。内部类是一种编译器行为，编译后还是一个普通的java类。
(2)为什么是静态而不是匿名什么的？
静态内部类和非静态内部类一个重要的区别就是内部类是否拥有外部类的引用，有这需求则选择非静态内部类，否则选择静态内部类。
```

#### 招式演进

* Step 1

```java
public class BankAccount {
    private long accountNumber;
    private String owner;
    private double balance;
    public BankAccount(long accountNumber, String owner, double balance) {
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.balance = balance;
    }
    //Getters and setters omitted for brevity.
}

BankAccount account = new BankAccount(123L, "Bart", 100.00);
```

* Step 2
```java
public class BankAccount {
    private long accountNumber;
    private String owner;
    private String branch;
    private double balance;
    private double interestRate;
    public BankAccount(long accountNumber, String owner, String branch, double balance, double interestRate) {
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.branch = branch;
        this.balance = balance;
        this.interestRate = interestRate;
   }
    //Getters and setters omitted for brevity.
}


BankAccount account = new BankAccount(456L, "Marge", "Springfield", 100.00, 2.5);
BankAccount anotherAccount = new BankAccount(789L, "Homer", null, 2.5, 100.00);  //Oops!
```
* Step3

```java
public class BankAccount {
    public static class Builder {
        private long accountNumber; //This is important, so we'll pass it to the constructor.
        private String owner;
        private String branch;
        private double balance;
        private double interestRate;
        public Builder(long accountNumber) {
            this.accountNumber = accountNumber;
        }
        public Builder withOwner(String owner){
            this.owner = owner;
            return this;  //By returning the builder each time, we can create a fluent interface.
        }
        public Builder atBranch(String branch){
            this.branch = branch;
            return this;
        }
        public Builder openingBalance(double balance){
            this.balance = balance;
            return this;
        }
        public Builder atRate(double interestRate){
            this.interestRate = interestRate;
            return this;
        }
        public BankAccount build(){
            //Here we create the actual bank account object, which is always in a fully initialised state when it's returned.
            BankAccount account = new BankAccount();  //Since the builder is in the BankAccount class, we can invoke its private constructor.
            account.accountNumber = this.accountNumber;
            account.owner = this.owner;
            account.branch = this.branch;
            account.balance = this.balance;
            account.interestRate = this.interestRate;
            return account;
        }
    }
    //Fields omitted for brevity.
    private BankAccount() {
        //Constructor is now private.
    }
    //Getters and setters omitted for brevity.
}


BankAccount account = new BankAccount.Builder(1234L)
            .withOwner("Marge")
            .atBranch("Springfield")
            .openingBalance(100)
            .atRate(2.5)
            .build();
BankAccount anotherAccount = new BankAccount.Builder(4567L)
            .withOwner("Homer")
            .atBranch("Springfield")
            .openingBalance(100)
            .atRate(2.5)
            .build();

  
```