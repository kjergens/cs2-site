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

---

## Homework 10: Introduction to Objects

!!! attention

    *Assigned Class 21 · Due Class 22*

    ### Part 1: Classes and Objects

    1. For each item below, write *class* or *object*:
       - a) The concept of a "Dog" in general — a description of what every dog has and can do
       - b) A specific dog named Fido who is 3 years old and is a Labrador
       - c) The description of what every Student has: a name, a GPA, and a grade level
       - d) The particular student with name "Alex" and GPA 3.8

    2. A class has two kinds of members — **attributes** (instance variables) and **behaviors** (methods). For a `BankAccount` class, sort these into Attributes vs. Behaviors: current balance, deposit money, account holder's name, withdraw money, account number, calculate interest.

    3. A program can create many `Dog` objects, each with its own name, breed, and age. Does each object get its own separate copy of the instance variables, or do all objects share one copy?

    ### Part 2: Reading a Class Definition

    ```java
    public class Dog {
        public String name;
        public String breed;
        public int age;

        public void bark() {
            System.out.println(name + " says: Woof!");
        }

        public void birthday() {
            age = age + 1;
            System.out.println("Happy birthday, " + name + "!");
        }
    }
    ```

    4. List all the instance variables of the `Dog` class. For each one, give its type.
    5. List all the methods of the `Dog` class.
    6. The `bark` method uses the variable `name` without declaring it inside the method. Where does `name` come from?
    7. The `birthday` method modifies `age`. If you call `birthday()` on a `Dog` object, does the change persist — is the object's `age` actually different afterward? Explain briefly.

    ### Part 3: Creating and Using Objects

    ```java
    public static void main(String[] args) {
        Dog fido = new Dog();
        fido.name = "Fido";
        fido.breed = "Labrador";
        fido.age = 3;

        Dog rex = new Dog();
        rex.name = "Rex";
        rex.breed = "German Shepherd";
        rex.age = 5;

        fido.bark();
        rex.bark();
        fido.birthday();
        System.out.println(fido.age);
    }
    ```

    8. What does `new Dog()` do?
    9. What does `fido.name = "Fido"` do? How is this different from declaring a local variable?
    10. Trace the program. Write the output in order.
    11. After the program finishes, what is the value of `rex.age`? Explain why.

    ### Part 4: Write a Class

    12. Define a class called `Rectangle` with two `double` instance variables (`width`, `height`), a void method `describe` that prints `"Rectangle: W x H"` (substituting the actual values), and a method `area` that returns `width * height` as a `double` (no printing inside).

    13. Write a `main` method (in a separate class, not inside `Rectangle`) that creates a `Rectangle` with width `5.0` and height `3.0`, calls `describe()` on it, stores the result of `area()` in a variable, and prints it.
