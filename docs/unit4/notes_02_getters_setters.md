# CS2 — Unit Notes: Getters and Setters

---

## 1. Reading Private Fields: Getters

When instance variables are `private`, outside code can't read them directly. A **getter** (also called an *accessor method*) is a public method that returns the value of a private field.

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

## 2. Writing Private Fields: Setters

A **setter** (also called a *mutator method*) is a public void method that sets the value of a private field. The key advantage over direct assignment: a setter can *validate* the new value before accepting it.

```java
public void setBalance(double newBalance) {
    if (newBalance >= 0) {
        balance = newBalance;
    }
}
```

Now setting a negative balance is silently rejected. The object stays in a valid state.

**Naming convention:** setter methods are named `set` + the field name with a capital first letter: `setBalance`, `setOwner`, `setName`.

---

## 3. Validation in Setters

Setters can enforce any rule the class needs:

```java
public class Student {
    private String name;
    private double gpa;

    public void setGpa(double newGpa) {
        if (newGpa >= 0.0 && newGpa <= 4.0) {
            gpa = newGpa;
        }
    }

    public void setName(String newName) {
        if (newName != null && newName.length() > 0) {
            name = newName;
        }
    }
}
```

If the validation fails, the field is simply not updated. The caller gets no error message here — that's a design choice for an intro course. (In production code you'd throw an exception, but that's beyond CS2.)

---

## 4. When NOT to Write a Setter

Not every field needs a setter. If a field should never change after construction, don't write a setter for it — the constructor sets it once, and that's final.

```java
public class BankAccount {
    private String owner;    // no setOwner — accounts don't change hands
    private double balance;  // no setBalance directly — use deposit/withdraw

    // Only getters — no setters
    public String getOwner()   { return owner; }
    public double getBalance() { return balance; }
}
```

The question to ask: "Should outside code ever be able to change this field?" If the answer is no, don't write a setter.

---

## 5. The Full Pattern

```java
public class Dog {
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    // Getters
    public String getName() { return name; }
    public int getAge()     { return age; }

    // Setters (with validation)
    public void setName(String newName) {
        if (newName != null && newName.length() > 0) {
            name = newName;
        }
    }

    public void setAge(int newAge) {
        if (newAge >= 0) {
            age = newAge;
        }
    }

    public String toString() {
        return name + " (age " + age + ")";
    }
}
```

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `public double getBalance() { balance = newBalance; }` | Getter that modifies — contradicts its purpose | Getters only `return`, never assign |
| `public void setAge(int newAge) { return newAge; }` | Setters are void — they assign, don't return | Remove `return newAge`; assign instead: `age = newAge;` |
| Setter with no validation | Works, but misses the point | Add an `if` to reject invalid values |
| `getBalance` vs `getbalance` | Compile error (wrong name) | Convention: capital letter after `get` |

---

## Check Your Understanding

Given:
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

**1.** Write a getter for `width` and a getter for `height`.

**2.** Write a setter for `width` that rejects zero or negative values.

**3.** Should `Rectangle` have a setter for `height`? Does it depend on anything?

**4.** Add a non-void method `area()` that returns width × height. Is this a getter? Why or why not?

---
---

## Answer Key

**1.**
```java
public double getWidth()  { return width; }
public double getHeight() { return height; }
```

**2.**
```java
public void setWidth(double newWidth) {
    if (newWidth > 0) {
        width = newWidth;
    }
}
```

**3.** It depends on the design. If rectangles are supposed to be resizable, yes. If they're immutable (fixed at construction), no. The class designer makes this call.

**4.** `area()` is not a getter — it computes a derived value rather than returning a stored field directly. It's a non-void instance method, but not an accessor in the strict sense.

---

## Homework 13: Private Fields and Getters

!!! attention

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

    ---

## Homework 14: Setters

!!! attention

    *Assigned Class 27 · Due Class 28*

    ### Part 1: Reading Setters

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

        public String getName()  { return name; }
        public int getGrade()    { return grade; }
        public double getGpa()   { return gpa; }

        public void setName(String name) {
            this.name = name;
        }

        public void setGrade(int grade) {
            if (grade >= 9 && grade <= 12) {
                this.grade = grade;
            }
        }

        public void setGpa(double gpa) {
            if (gpa >= 0.0 && gpa <= 4.0) {
                this.gpa = gpa;
            }
        }

        public String toString() {
            return name + " (Grade " + grade + ", GPA: " + gpa + ")";
        }
    }
    ```

    1. Which setters include validation? What rule does each one enforce?
    2. What happens when you call `s.setGpa(4.8)` on a `Student` whose current GPA is 3.5? What is the student's GPA afterward?
    3. `setName` has no validation — it accepts any String. Why might it still be worth having a setter rather than making `name` public?
    4. The constructor sets `gpa` directly with `this.gpa = gpa`, bypassing `setGpa`. This means the constructor could store an invalid GPA. Describe one way to fix this (you don't need to write code).

    ### Part 2: Predict the Output

    5.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Alex", 10, 3.2);
        s.setGpa(3.8);
        s.setGrade(11);
        System.out.println(s.toString());
    }
    ```

    6.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Jordan", 11, 3.6);
        s.setGpa(-1.0);
        s.setGrade(13);
        System.out.println(s.getGpa());
        System.out.println(s.getGrade());
    }
    ```

    7.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Casey", 9, 2.9);
        s.setGpa(3.5);
        System.out.println(s.getGpa());
        s.setGpa(5.0);
        System.out.println(s.getGpa());
        s.setName("Casey M.");
        System.out.println(s.getName());
    }
    ```

    ### Part 3: Write Setters

    8. Here is a `Circle` class. Add a setter for `radius` that rejects any value that is zero or negative (if the value is invalid, do nothing).
    ```java
    public class Circle {
        private double radius;

        public Circle(double radius) {
            this.radius = radius;
        }

        public double getRadius() { return radius; }

        // Write setRadius here
    }
    ```

    9. Here is a partial `BankAccount` class. Add two setters: `setOwner(String owner)` — rejects an empty string (if owner equals `""`, do nothing) — and `deposit(double amount)` — adds `amount` to `balance`, but only if `amount` is greater than zero.
    ```java
    public class BankAccount {
        private String owner;
        private double balance;

        public BankAccount(String owner, double initialBalance) {
            this.owner = owner;
            this.balance = initialBalance;
        }

        public String getOwner()  { return owner; }
        public double getBalance() { return balance; }

        // Write setOwner and deposit here
    }
    ```

    10. Write a `main` that creates a `BankAccount` with owner `"Morgan"` and balance `200.0`, deposits `50.0`, tries to deposit `-100.0` (invalid — should be ignored), and prints the balance.

    ### Part 4: Find the Bug

    11.
    ```java
    public double setRating(double rating) {
        if (rating >= 0.0 && rating <= 10.0) {
            this.rating = rating;
        }
        return rating;
    }
    ```

    12. Compiles with no errors — but something is wrong. What?
    ```java
    public void setName(String name) {
        name = name;
    }
    ```

    13. Compiles with no errors — but the validation is wrong. What does it actually do, and what should it do instead?
    ```java
    public void setAge(int age) {
        if (age < 0) {
            this.age = age;
        }
    }
    ```
