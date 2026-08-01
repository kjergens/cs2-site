# CS2 — Unit Notes: Introduction to Objects

---

## 1. What Is Object-Oriented Programming?

So far every program you've written has been a list of static methods that work with primitive data (ints, doubles) and arrays. **Object-oriented programming** is a different way of organizing code: you define your own data types, called **classes**, that bundle data and behavior together.

A **class** is a blueprint. An **object** is one instance built from that blueprint.

```java
// Blueprint (class) — defined once
public class Dog {
    String name;
    String breed;
    int age;
}

// Instances (objects) — created as many times as you want
Dog d1 = new Dog();
Dog d2 = new Dog();
```

`d1` and `d2` are two separate dogs. Each has its own `name`, `breed`, and `age`. The class defines what fields exist; each object holds its own values for those fields.

---

## 2. Attributes vs. Behaviors

Every class has two kinds of members:

| Member | Also called | What it is |
|---|---|---|
| **Instance variable** | attribute, field | A piece of data each object holds |
| **Method** | behavior | Something each object can do |

```java
public class BankAccount {
    String owner;    // instance variable — whose account
    double balance;  // instance variable — how much money

    // methods (behaviors) come later — deposit, withdraw, etc.
}
```

The instance variables live inside the object. Each `BankAccount` object has its own `owner` and its own `balance`.

---

## 3. Defining a Class

```java
public class ClassName {
    // instance variables (attributes)
    type variableName;
    type variableName;

    // methods (behaviors) — covered in Class 23
}
```

A class definition goes in its own file, named `ClassName.java`. The class name and the filename must match exactly.

**A simple example:**

```java
public class Dog {
    String name;
    String breed;
    int age;
}
```

This class has three attributes and no methods yet. It can still be used to create objects — we just can't ask the dog to do anything yet.

---

## 4. Creating Objects and Accessing Fields

To create an object, use the `new` keyword:

```java
Dog myDog = new Dog();
```

To access a field, use **dot notation** — object name, dot, field name:

```java
myDog.name = "Rex";
myDog.breed = "Labrador";
myDog.age = 3;

System.out.println(myDog.name);   // Rex
System.out.println(myDog.age);    // 3
```

Each object gets its own copy of every instance variable. Changing one object's field does not affect any other object.

```java
Dog dog1 = new Dog();
Dog dog2 = new Dog();

dog1.name = "Rex";
dog2.name = "Bella";

System.out.println(dog1.name);  // Rex
System.out.println(dog2.name);  // Bella
```

---

## 5. The BankAccount Class — Unit 3 Running Example

This unit builds a `BankAccount` class from scratch, adding features each class:

```java
public class BankAccount {
    String owner;
    double balance;
}
```

In `main` (or any other class), you create and use accounts:

```java
BankAccount acct = new BankAccount();
acct.owner = "Alice";
acct.balance = 500.0;

System.out.println(acct.owner + " has $" + acct.balance);
// Alice has $500.0
```

By Class 23, the same class will have constructors and methods. The starting point is always the same: define the class, list the instance variables.

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `Dog d = Dog();` | Missing `new` | Write `new Dog()` |
| `Dog.name = "Rex";` | Using class name instead of object name | Create an object: `Dog d = new Dog(); d.name = "Rex";` |
| Two classes in the same file (public) | Compile error | Each public class needs its own file |
| Filename doesn't match class name | Compile error | File must be `ClassName.java` |

---

## Check Your Understanding

**1.** What is the difference between a class and an object?

**2.** What are the two kinds of members a class can have?

**3.** Given:
```java
public class Car {
    String make;
    int year;
    double price;
}
```
Write code in `main` to create a `Car` object, set all three fields, and print the year.

**4.** You create two `Dog` objects, `d1` and `d2`, and set `d1.name = "Rex"`. Does `d2.name` change? Why or why not?

---
---

## Answer Key

**1.** A class is a blueprint — it defines what fields and methods exist. An object is one instance built from that blueprint. You can create many objects from the same class; each holds its own data.

**2.** Instance variables (attributes) and methods (behaviors).

**3.**
```java
Car myCar = new Car();
myCar.make = "Toyota";
myCar.year = 2020;
myCar.price = 24999.99;
System.out.println(myCar.year);
```

**4.** No. Each object has its own copy of every instance variable. Changing `d1.name` only affects `d1`.
