---
title: 0. Java OOP
updated: 2022-01-22 17:21:29Z
created: 2021-12-26 15:34:24Z
source: https://dev.to/sivantha96/java-oop-cheatsheet-3ph1
tags:
  - multi-paradigme
  - s1
---

# Encapsulation

## Definition

Wrapping the fields (state) and methods (behaviors) together as a single unit in a way that sensitive data are hidden from the users.

### in the real word
With an ATM, we can perform operations like cash withdrawal, money transfer, balance checks. But only the account owner can perform those operations. So, all the operations are encapsulated inside an object for that particular account owner. And the only way to access data like the balance is by using the predefined operations such as balance checks.

## Java

Using access modifiers like `private` and `protected` for class variables (fields) and providing getters and setters to access them if necessary.

```
public class Person {
    private String name; // using private access modifier

    // Getter
    public String getName() {
      return name;
    }

    // Setter
    public void setName(String newName) {
      this.name = newName;
    }
} 
```

## Why use encapsulation

1.  **Data Hiding** \- Data inside an encapsulated class can only be accessible inside the class or through a getter or a setter method.
2.  **Flexibility** \- We can make variables read-only or write-only by omitting the getter or setter of that variable
3.  **Reusability**
4.  **Easy to perform unit testing**

# Inheritance

## Definition

A mechanism where an object of the subclass (child class), acquires all the properties (fields) and behaviors (methods) of an object of the superclass (parent class)

## Java

By extending the child class to parent classes using `extends` or `implements` keywords.

```
// parent class
class Employee {
    float salary = 100; // property of the parent class
}

// child class
class Programmer extends Employee {
    float bonus = 30;
}

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Programmer myVariable = new Programmer()
        System.out.println("My salary is " + myVariable.salary );
        System.out.println("My bonus is " + myVariable.bonus );
    }
} 
```

The result will be

```
My salary is 100
My bonus is 30 
```

Here, `myVariable` object has access to the `salary` which belongs to the parent class even though `myVariable` is an object of the child class.

## Why use inheritance

- For code reusability
- To take advantage of polymorphism

# Polymorphism

## Definition

Perform a single action in different ways

### in the real word

A man can be a father, a husband, and an employee according to the situation he is in.

### Java

There are two types of polymorphism in java

1.  **Compile-time polymorphism** (Static polymorphism)
2.  **Run-time polymorphism** (Dynamic polymorphism)

## Compile-time polymorphism

When **method overloading** is used, which method to call is resolved during the compile time by looking at the signature of the method invoke statement.

### method overloading

A feature that allows a class to have more than one method having the **same name** but with different signatures.

### signature of a method

The signature of a method is determined by the **number of arguments**, **types of each argument**, and the **order of the arguments**. The return type of the method does not affect the signature.

```
// A class with multiple methods with the same name
public class Adder {
    // method 1
    public void add(int a, int b) {
        System.out.println(a + b);
    }

    // method 2
    public void add(int a, int b, int c) {
        System.out.println(a + b + c);
    }

    // method 3
    public void add(String a, String b) {
        System.out.println(a + " + " + b);
    }
}

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Adder adder = new Adder(); // create a Adder object
        adder.add(5, 4); // invoke method 1
        adder.add(5, 4, 3); // invoke method 2
        adder.add("5", "4"); // invoke method 3
    }
} 
```

The result will be

```
9
12
5 + 4 
```

## Run-time polymorphism

When **method overriding** and **upcasting** is used, which class’s method to call is resolved during the run-time.

### method overriding

Providing a specific implementation in the child class (subclass) of a method that already provided by one of its parent classes (super-classes). And also, the method in the subclass should have the **same signature** and the **same return type** for it to override the superclass method.

### upcasting

When the reference variable of the parent class is referring to an object of the child class, it is upcasting. In other words, casting an object of an individual type to one common type.

### downcasting

When the reference variable of the child class is referring to an object of the parent class, it is downcasting. In other words, casting an object of a common type to a narrower (special) type.

### Real-life example of Upcasting & Downcasting

Assume there are classes named, `Fruits` and `Apple`. And the `Apple` class is the child class of the `Fruits` class.

If we cast an object of the `Apple` class to `Fruits` type, it is Upcasting. If we cast an object of the `Fruits` class to `Apple` type, it is Downcasting

## Remarks

- Upcasting can be done directly in Java. But Downcasting cannot. We have to do the casting **explicitly**.
- But we have to be careful not to downcast incompatible types. Unless it will throw an error.

```
class Fruits {} // parent class
class Apple extends Fruits {} // child class

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Fruits upcastedVariable = new Apple() // upcasting (implicit casting)
        Apple downcastedVariable = (Apple) upcastedVariable // downcasting (explicit casting)
    }
} 
```

Here `upcastedVariable` can be downcasted into `Apple` because, even though `upcastedVariable` is `Fruit` type, it refers to an object of the `Apple` class.

```
class Fruits {} // parent class
class Apple extends Fruits {} // child class

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Fruits myVariable = new Fruits()
        Apple downcastedVariable = (Apple) myVariable // throws an error
    }
} 
```

But in the above example, `myVariable` is `Fruit` type as well as it refers to a `Fruit` object. Therefore when we downcast into the `Apple` type it will throw an error.

```
// parent class
class Bank {
    public float getInterestRate() {
        return 0
    }
}

// child class - 1
class AwesomeBank extends Bank {
    public float getInterestRate() { // superclass's method is overridden
        return 8.4;
    }
}

// child class - 2
class SuperBank extends Bank {
    public float getInterestRate() { // superclass's method is overridden
        return 9.6;
    }
}

// child class - 3
class GovernmentBank extends Bank {} // superclass's method is not overridden

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Bank b = new Bank()
        System.out.println(b.getInterestRate());
        b = new AwesomeBank() // upcasting
        System.out.println(b.getInterestRate());
        b = new SuperBank() // upcasting
        System.out.println(b.getInterestRate());
        b = new GovernmentBank() // upcasting
        System.out.println(b.getInterestRate());
    }
} 
```

The result will be

```
0
8.4
9.6
0 
```

Since in `AwesomeBank` class the `getInterestRate()` method is not overridden, the method in the parent class will be called in the run-time.

# Abstraction

## Definition

Hiding certain details and showing only the essential information to the user. In other words, identifying only the required characteristics of an object ignoring the irrelevant details.

## in the real word

With an ATM, we can perform operations like cash withdrawal, money transfer, balance checks, etc. But we can’t know the internal details about those operations, how they operate or likewise.

## Java

Using either an `abstract class` or an `interface`

### abstract class

A class contains both abstract and regular methods.

### abstract method

A method without a body (i.e. no implementation). The implementation is provided by the subclass that inherits the abstract class

```
// abstract class
abstract class Animal {
    public abstract void animalSound(); // abstract method
    public void sleep() {
        System.out.println("Zzzzzz!")
    }
} 
```

## Remarks

- `abstract` keyword is a non-access modifier, used for classes and methods.
- You don’t have to implement (override) all methods of an abstract class. But you **must implement all the abstract methods** in it.
- You cannot create objects from an abstract class
- To access an abstract class you have to inherit (`extend`) it from another class
- Fields in an abstract class should **not have to be** `public`, `static`, and `final`
- You can have `public`, `private` or `protected` methods

```
// abstract class
abstract class Animal {
    public abstract void animalSound(); // abstract method
    public void sleep() {
        System.out.println("Zzzzzz!")
    }
}

// subclass (inherits the abstract class)
class Pig extends Animal {
    public void AnimalSound() { // body of the abstract method is provided here
        System.out.println("Wee Wee!");
    }
}

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Pig myPig = new Pig(); // create a Pig object
        myPig.animalSound();
        myPig.sleep();
    }
} 
```

The results will be

```
Wee Wee!
Zzzzzz! 
```

## Why use Abstract classes

To have some methods to implement later. If you don’t use an abstract class, you have to implement the `AnimalSound()` method in the `Animal` class even if you inherit it from another class.

# Interface

## What is an interface

A completely abstract class. In other words, all the methods in an interface should not have a body.

```
// interface
interface Animal {
    public void animalSound(); // interface method
    public void sleep(); // interface method
} 
```

## Remarks

- You cannot create objects from an interface.
- To access an interface you have to `implement` (kinda like inherit) it from another class.
- You must override all the methods in an interface from a subclass.
- All the fields in an interface are `public`, `static`, and `final`.
- All the methods are `public`

```
// interface
interface Animal {
    public void animalSound(); // interface method
    public void sleep(); // interface method
}

// subclass (implemets the abstract class)
class Cat implements Animal {
    public void AnimalSound() { // body of the interface method is provided here
        System.out.println("Meow!");
    }
    public void sleep() { // body of the interface method is provided here
        System.out.println("Purrrrr!");
    }

}

// My main class
class MyMainClass {
    public static void main(String[] args) {
        Cat myCat = new Cat(); // create a Cat object
        myCat.animalSound();
        myCat.sleep();
    }
} 
```

The results will be

```
Meow!
Purrrrr! 
```

# Why use Interfaces?

- To specify the behavior of a particular data
- To achieve **multiple inheritance**.

> Java doesn’t support multiple inheritances. ( You can only inherit from a single superclass.) However, it can be achieved using interfaces.