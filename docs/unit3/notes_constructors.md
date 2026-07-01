# CS2 — Unit Notes: Constructors and Creating Objects

---

## 1. The Problem with Direct Field Assignment

In the last class you created objects like this:

```java
BankAccount acct = new BankAccount();
acct.owner = "Alice";
acct.balance = 500.0;
```

This works, but it's fragile. Nothing forces you to set both fields. You could forget `acct.balance` and end up with an account that starts at 0.0 by default — or worse, forget `acct.owner` entirely. A **constructor** solves this by initializing fields the moment the object is created.

---

## 2. Constructor Syntax

A constructor looks like a method, with two differences:
1. It has **no return type** (not even `void`)
2. Its name **must match the class name exactly**

```java
public class BankAccount {
    String owner;
    double balance;

    public BankAccount(String ownerName, double startingBalance) {
        owner = ownerName;
        balance = startingBalance;
    }
}
```

Now you can create a fully initialized account in one line:

```java
BankAccount acct = new BankAccount("Alice", 500.0);
```

The arguments `"Alice"` and `500.0` are passed to the constructor's parameters, which then set the instance variables.

---

## 3. The `this` Keyword

When a constructor parameter has the same name as an instance variable, use `this` to tell them apart. `this.owner` means "the instance variable of this object"; `owner` alone refers to the parameter.

```java
public BankAccount(String owner, double balance) {
    this.owner = owner;     // this.owner = instance variable
    this.balance = balance; // balance (right side) = parameter
}
```

Both versions work — using matching names with `this` is a common Java convention.

---

## 4. Creating Objects with `new`

```java
BankAccount alice = new BankAccount("Alice", 500.0);
BankAccount bob   = new BankAccount("Bob",   200.0);

System.out.println(alice.owner);    // Alice
System.out.println(bob.balance);    // 200.0
```

`new` allocates memory for the object and runs the constructor. After that line, the object is ready to use.

Each object has its own copies of all instance variables. `alice` and `bob` are completely independent.

---

## 5. The Default Constructor

If you write **no constructor at all**, Java provides a default one with no parameters. That's what let `new BankAccount()` work in the last class. Once you write your own constructor, the default goes away — you must call your constructor (with its required arguments) or Java will refuse to compile.

```java
// If BankAccount has a (String, double) constructor:
BankAccount acct = new BankAccount();           // ✗ compile error — no arg constructor
BankAccount acct = new BankAccount("Alice", 0); // ✓
```

---

## 6. Multiple Constructors

A class can have more than one constructor, as long as their parameter lists differ (overloading, just like regular methods):

```java
public BankAccount(String owner, double balance) {
    this.owner = owner;
    this.balance = balance;
}

public BankAccount(String owner) {
    this.owner = owner;
    this.balance = 0.0;   // default starting balance
}
```

`new BankAccount("Alice", 500.0)` calls the first. `new BankAccount("Bob")` calls the second and starts Bob with $0.

---

## 7. Common Errors

| Error | Problem | Fix |
|---|---|---|
| Constructor has a return type | Treated as a regular method, not a constructor | Remove the return type |
| Constructor name doesn't match class name | Treated as a regular method | Fix the name to match exactly |
| `new BankAccount()` after defining a `(String, double)` constructor | Compile error — default gone | Call your constructor with arguments |
| Setting `owner` but forgetting `balance` | Balance stays 0.0 (default) | Set both in the constructor |

---

## Check Your Understanding

**1.** What are the two things that make a constructor different from a regular method?

**2.** What does `this` mean inside a constructor?

**3.** Write a `Car` class with instance variables `make` (String) and `year` (int). Write a constructor that takes both values as parameters and initializes the instance variables.

**4.** Given your `Car` class, write code in `main` to create two different cars and print each car's make.

**5.** If a class has a constructor `public Dog(String name, String breed)`, will `new Dog()` compile? Explain.

---
---

## Answer Key

**1.** No return type (not even void); name must match the class name exactly.

**2.** `this` refers to the current object. `this.fieldName` accesses the instance variable, distinguishing it from a parameter with the same name.

**3.**
```java
public class Car {
    String make;
    int year;

    public Car(String make, int year) {
        this.make = make;
        this.year = year;
    }
}
```

**4.**
```java
Car c1 = new Car("Toyota", 2020);
Car c2 = new Car("Honda", 2018);
System.out.println(c1.make);
System.out.println(c2.make);
```

**5.** No. Once you define a constructor with parameters, the no-argument default constructor disappears. `new Dog()` would cause a compile error.
