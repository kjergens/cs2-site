# CS2 — Unit Notes: Instance Methods

---

## 1. Adding Behavior to a Class

An **instance method** is a method defined inside a class. It runs on a specific object, and has access to all of that object's instance variables.

So far, `BankAccount` has data but no behavior:

```java
public class BankAccount {
    String owner;
    double balance;

    public BankAccount(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
    }
}
```

Adding methods gives the account things it can *do* — deposit, withdraw, report its balance.

---

## 2. Writing an Instance Method

Instance methods go inside the class body, alongside the constructor. They look almost like the static methods from Unit 2, but without the `static` keyword.

```java
public class BankAccount {
    String owner;
    double balance;

    public BankAccount(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
    }

    public void deposit(double amount) {
        balance = balance + amount;
    }

    public void withdraw(double amount) {
        if (amount <= balance) {
            balance = balance - amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

Notice: instance methods access `balance` and `owner` directly — they're inside the class, so those fields are automatically available.

---

## 3. Calling Methods via Dot Notation

In `main` (or any class outside `BankAccount`), call methods using dot notation — object, dot, method name, arguments:

```java
BankAccount acct = new BankAccount("Alice", 500.0);

acct.deposit(200.0);          // balance is now 700.0
acct.withdraw(150.0);         // balance is now 550.0
System.out.println(acct.getBalance()); // 550.0
```

Each method call runs on `acct`'s data. If you had a second account `acct2`, calling `acct2.deposit(100.0)` would only change `acct2`'s balance.

---

## 4. Void vs. Non-Void Instance Methods

The same rules from Unit 2 apply:

| Type | Returns | When to use |
|---|---|---|
| `void` | Nothing | Method changes the object's state |
| non-void | A value | Method computes something from the object's data |

`deposit` and `withdraw` are void — they modify `balance` in place.  
`getBalance` returns a double — it reads `balance` without changing it.

---

## 5. The `toString` Method

`toString` is a special method that returns a String description of the object. Java calls it automatically when you pass an object to `System.out.println`.

```java
public String toString() {
    return owner + ": $" + balance;
}
```

Now:
```java
BankAccount acct = new BankAccount("Alice", 500.0);
System.out.println(acct);     // Alice: $500.0
```

Without `toString`, `System.out.println(acct)` would print something like `BankAccount@3d04a311` — a memory address. Defining `toString` makes your objects print usefully.

---

## 6. The Full BankAccount Class

```java
public class BankAccount {
    String owner;
    double balance;

    public BankAccount(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
    }

    public void deposit(double amount) {
        balance += amount;
    }

    public void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
        }
    }

    public double getBalance() {
        return balance;
    }

    public String toString() {
        return owner + ": $" + balance;
    }
}
```

---

## 7. Calling One Object's Methods from Another

Methods can take objects as parameters. If you want to transfer money between two accounts:

```java
public void transferTo(BankAccount other, double amount) {
    if (amount <= balance) {
        balance -= amount;
        other.balance += amount;
    }
}
```

Usage:
```java
BankAccount alice = new BankAccount("Alice", 500.0);
BankAccount bob   = new BankAccount("Bob",   200.0);
alice.transferTo(bob, 100.0);
System.out.println(alice);   // Alice: $400.0
System.out.println(bob);     // Bob: $300.0
```

---

## 8. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `public static void deposit(...)` | `static` methods can't access instance variables | Remove `static` |
| `acct.deposit` without parentheses | Compile error — missing method call syntax | Write `acct.deposit(amount)` |
| Forgetting `toString` and printing the object | Prints memory address | Define `public String toString()` |
| Calling an instance method without an object | `deposit(100.0)` in `main` — compile error | Need `acct.deposit(100.0)` |

---

## Check Your Understanding

Given this class:

```java
public class Counter {
    int count;

    public Counter() {
        count = 0;
    }

    public void increment() {
        count++;
    }

    public void reset() {
        count = 0;
    }

    public int getCount() {
        return count;
    }

    public String toString() {
        return "Counter: " + count;
    }
}
```

**1.** What does `increment` do? What type does it return?

**2.** Trace this code and state the output:
```java
Counter c = new Counter();
c.increment();
c.increment();
c.increment();
c.reset();
c.increment();
System.out.println(c.getCount());
System.out.println(c);
```

**3.** Add a method `decrementByN(int n)` that subtracts `n` from `count` but never lets `count` go below 0.

---
---

## Answer Key

**1.** `increment` adds 1 to `count`. It is a void method — it changes the object's state and returns nothing.

**2.**
- After three increments: `count = 3`
- After reset: `count = 0`
- After one increment: `count = 1`
- `getCount()` returns `1` → prints `1`
- `toString()` returns `"Counter: 1"` → prints `Counter: 1`

**3.**
```java
public void decrementByN(int n) {
    count = count - n;
    if (count < 0) {
        count = 0;
    }
}
```

---

## Homework 12: Instance Methods

!!! attention

    **Unit 3 · Chapter 3**

    *Assigned Class 23 · Due Class 24*

    ### Part 1: Reading Instance Methods

    ```java
    public class Student {
        public String name;
        public int grade;
        public double gpa;

        public Student(String name, int grade, double gpa) {
            this.name = name;
            this.grade = grade;
            this.gpa = gpa;
        }

        public void promote() {
            grade++;
        }

        public void adjustGpa(double change) {
            gpa = gpa + change;
        }

        public boolean isHonorRoll() {
            return gpa >= 3.5;
        }

        public String toString() {
            return name + " (Grade " + grade + ", GPA: " + gpa + ")";
        }
    }
    ```

    1. Which methods are void? Which return a value — and what type?
    2. Notice that none of these methods have the keyword `static`. How does that change the way you call them?
    3. The `isHonorRoll` method uses `gpa` without declaring it or passing it as a parameter. Why does this work?
    4. `toString()` is a special method name in Java. When you pass an object to `System.out.println`, Java automatically calls its `toString()` method. What does the following line print?
    ```java
    System.out.println(new Student("Jordan", 11, 3.7));
    ```

    ### Part 2: Predict the Output

    5.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Alex", 9, 3.2);
        System.out.println(s.isHonorRoll());
        s.adjustGpa(0.4);
        System.out.println(s.isHonorRoll());
        System.out.println(s.gpa);
    }
    ```

    6.
    ```java
    public static void main(String[] args) {
        Student s1 = new Student("Morgan", 10, 3.8);
        Student s2 = new Student("Riley", 10, 3.1);
        s1.promote();
        System.out.println(s1.toString());
        System.out.println(s2.toString());
    }
    ```

    7.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Casey", 12, 3.6);
        System.out.println(s.isHonorRoll());
        s.adjustGpa(-0.2);
        System.out.println(s.isHonorRoll());
        s.promote();
        System.out.println(s.toString());
    }
    ```

    ### Part 3: Write Instance Methods

    Use this class for problems 8–10:
    ```java
    public class Rectangle {
        public double width;
        public double height;

        public Rectangle(double width, double height) {
            this.width = width;
            this.height = height;
        }
    }
    ```

    8. Add a method `area` that returns `width * height` as a `double`.
    9. Add a method `perimeter` that returns `2 * (width + height)` as a `double`.
    10. Add a `toString` method that returns a String in this format: `"Rectangle: W x H"` (e.g. `"Rectangle: 4.0 x 3.0"`). Then write a `main` that creates a `Rectangle` with width `4.0` and height `3.0`, prints it directly with `System.out.println` (which calls `toString` automatically), and prints its area.

    ### Part 4: Find the Bug

    11.
    ```java
    public class Thermometer {
        public double tempC;

        public Thermometer(double tempC) {
            this.tempC = tempC;
        }

        public static double toFahrenheit() {
            return tempC * 9.0 / 5.0 + 32;
        }
    }
    ```

    12.
    ```java
    public class Circle {
        public double radius;

        public Circle(double radius) {
            this.radius = radius;
        }

        public double area() {
            System.out.println(Math.PI * radius * radius);
        }
    }
    ```

    13.
    ```java
    public class Counter {
        public int count;

        public Counter(int start) {
            count = start;
        }

        public void increment() {
            count++;
        }
    }

    public static void main(String[] args) {
        Counter.increment();
        System.out.println(Counter.count);
    }
    ```
