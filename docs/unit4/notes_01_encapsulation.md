# CS2 — Unit Notes: Encapsulation and Getters

---

## 1. The Problem with Public Fields

In Unit 3, all instance variables were public — any code could read or write them directly:

```java
BankAccount acct = new BankAccount("Alice", 500.0);
acct.balance = -99999.0;   // valid Java — nothing stops this
```

There is nothing wrong with this *syntactically*. But it means any piece of code anywhere in the program can put a bank account into an invalid state. The class itself has no say.

**Encapsulation** is the principle of hiding an object's internal data so that the object controls its own state. The mechanism in Java is the `private` keyword.

---

## 2. The `private` Access Modifier

| Modifier | Who can access it |
|---|---|
| `public` | Any code, anywhere |
| `private` | Only code inside the same class |

Making instance variables `private` means the only way to read or modify them from outside the class is through methods the class provides:

```java
public class BankAccount {
    private String owner;    // private — can't be set from outside
    private double balance;  // private — can't be set from outside

    public BankAccount(String owner, double balance) {
        this.owner   = owner;
        this.balance = balance;
    }
}
```

Now `acct.balance = -99999.0` is a **compile error**. The field is not accessible from outside `BankAccount`.

---

## 3. Why Encapsulate?

**Control over state:** The class gets to decide what values are valid. A `withdraw` method can check that the amount is positive and doesn't exceed the balance — direct field access can't.

**Protection from bugs:** Other parts of the program can't accidentally corrupt an object's state by assigning to a field they shouldn't touch.

**Flexibility to change implementation:** If you decide to store balance in cents (as an int) instead of dollars (as a double), you can change the private field without breaking any code that uses the class — as long as the public methods still work the same way.

---

## 4. What Happens Inside the Class

Code inside `BankAccount` can still access `private` fields directly:

```java
public void deposit(double amount) {
    balance += amount;     // ✓ — this code IS inside BankAccount
}
```

And the constructor works as before — it's inside the class:

```java
public BankAccount(String owner, double balance) {
    this.owner   = owner;   // ✓
    this.balance = balance; // ✓
}
```

Only code *outside* the class is blocked from touching private fields.

---

## 5. Reading Private Fields: Getters

Making a field `private` solves the "anyone can corrupt it" problem — but now outside code can't read it either. A **getter** (also called an *accessor method*) is a public method that returns the value of a private field, so outside code has a safe, controlled way to read it back.

```java
public class BankAccount {
    private String owner;
    private double balance;

    public String getOwner() {
        return owner;
    }

    public double getBalance() {
        return balance;
    }
}
```

Usage from outside the class:
```java
BankAccount acct = new BankAccount("Alice", 500.0);
System.out.println(acct.getOwner());    // Alice
System.out.println(acct.getBalance());  // 500.0
```

**Naming convention:** getter methods are named `get` + the field name with a capital first letter: `getOwner`, `getBalance`, `getName`, `getAge`.

---

## 6. The Encapsulated BankAccount

```java
public class BankAccount {
    private String owner;
    private double balance;

    public BankAccount(String owner, double balance) {
        this.owner   = owner;
        this.balance = balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
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

`getBalance()` is the only way for outside code to read the balance. There is no `setBalance()` — the balance only changes through `deposit` and `withdraw`, which enforce the rules. (Setters get their own dedicated treatment in the next chapter.)

---

## 7. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `acct.balance = 100` from outside the class | Compile error — field is private | Use a method: `acct.deposit(100)` |
| `acct.balance` in a print statement from outside | Compile error — field is private | Call `acct.getBalance()` instead |
| Forgetting `private` on some fields | Inconsistent encapsulation — some fields accessible, others not | Make all instance variables `private` |
| `public double getBalance() { balance = newBalance; }` | Getter that modifies — contradicts its purpose | Getters only `return`, never assign |
| `getBalance` vs `getbalance` | Compile error (wrong name) | Convention: capital letter after `get` |

---

## Check Your Understanding

**Unit 4 · Chapter 1**

**1.** What does `private` mean for an instance variable?

**2.** Why is it better to make instance variables `private` rather than leaving them `public`?

**3.** Given a `private double balance` field, which of the following will compile if written *outside* the class?
- `acct.balance = 100.0`
- `acct.deposit(100.0)`
- `System.out.println(acct.balance)`
- `System.out.println(acct.getBalance())`

**4.** Can a method inside `BankAccount` access `private double balance`? Why or why not?

**5.** Given:
```java
public class Rectangle {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width  = width;
        this.height = height;
    }
}
```
Write a getter for `width` and a getter for `height`.

**6.** Add a non-void method `area()` to `Rectangle` that returns width × height. Is this a getter? Why or why not?

---
---

## Answer Key

**1.** `private` means only code inside the same class can access it. Code outside the class gets a compile error if it tries to read or write the field directly.

**2.** Private fields let the class control its own state. Methods can validate values before changing them; direct access allows any code to put the object into an invalid state.

**3.** Compile error: `acct.balance = 100.0` and `System.out.println(acct.balance)`. Compiles: `acct.deposit(100.0)` and `acct.getBalance()`.

**4.** Yes. The `private` restriction applies to code *outside* the class. Methods inside `BankAccount` can access `balance` freely.

**5.**
```java
public double getWidth()  { return width; }
public double getHeight() { return height; }
```

**6.** `area()` is not a getter — it computes a derived value rather than returning a stored field directly. It's a non-void instance method, but not an accessor in the strict sense.

---

## Homework 13: Private Fields and Getters

!!! attention

    **Unit 4 · Chapter 1**

    *Assigned Class 26 · Due Class 27*

    ### Part 1: Why Private?

    ```java
    public class BankAccount {
        public String owner;
        public double balance;

        public BankAccount(String owner, double initialBalance) {
            this.owner = owner;
            this.balance = initialBalance;
        }
    }
    ```

    1. With public fields, any code anywhere can do this:
    ```java
    BankAccount acct = new BankAccount("Alex", 500.0);
    acct.balance = -9999999.0;
    acct.owner = "";
    ```
    Why is this a problem? Give a specific reason for each field.

    2. Now the fields are changed to `private`. Will `acct.balance = -9999999.0` still compile? Why or why not?

    3. With private fields, how can code outside the class read the current balance? (You don't need to write code — just describe the approach.)

    ### Part 2: Reading a Class with Getters

    ```java
    public class Student {
        private String name;
        private int grade;
        private double gpa;

        public Student(String name, int grade, double gpa) {
            this.name = name;
            this.grade = grade;
            this.gpa = gpa;
        }

        public String getName() {
            return name;
        }

        public int getGrade() {
            return grade;
        }

        public double getGpa() {
            return gpa;
        }

        public boolean isHonorRoll() {
            return gpa >= 3.5;
        }

        public String toString() {
            return name + " (Grade " + grade + ", GPA: " + gpa + ")";
        }
    }
    ```

    4. The fields are private, yet the constructor sets them with `this.name = name`. Why doesn't this violate the private restriction?
    5. `toString()` uses `name`, `grade`, and `gpa` directly — no getters needed. Why can it access private fields without going through `getName()`, `getGrade()`, or `getGpa()`?
    6. Write the two lines of `main` code that read a student's name and GPA and print each one on its own line. Assume the variable is `Student s`.
    7. True or false — explain your answer.
       - a) `s.gpa = 4.0` will compile if `gpa` is private.
       - b) `s.getGpa()` will compile if `getGpa` is public.

    ### Part 3: Write Getters

    8. The class below is missing its getter methods. Add a getter for each private field, following the naming convention `getFieldName()`.
    ```java
    public class Movie {
        private String title;
        private int year;
        private double rating;

        public Movie(String title, int year, double rating) {
            this.title = title;
            this.year = year;
            this.rating = rating;
        }

        // Write the three getters here
    }
    ```

    9. Add a `toString` method to `Movie` that returns a String in this format: `"Inception (2010) — 8.8/10"` (substituting actual values).

    10. Write a `main` method that creates a `Movie` with title `"Parasite"`, year `2019`, and rating `8.6`, uses getters to print the title and year on separate lines, and prints the full object with `System.out.println` (which calls `toString` automatically).

    ### Part 4: Find the Bug

    11.
    ```java
    Student s = new Student("Morgan", 10, 3.8);
    System.out.println(s.name);
    System.out.println(s.gpa);
    ```

    12. This getter compiles with no errors. What is wrong with it?
    ```java
    public int getGrade() {
        grade++;
        return grade;
    }
    ```

    13.
    ```java
    public class Circle {
        private double radius;

        public Circle(double radius) {
            this.radius = radius;
        }

        private double getRadius() {
            return radius;
        }
    }

    public static void main(String[] args) {
        Circle c = new Circle(5.0);
        System.out.println(c.getRadius());
    }
    ```
