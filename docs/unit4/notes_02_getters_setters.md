# CS2 — Unit 4 Chapter 2: Setters

---

## 1. Writing Private Fields: Setters

You've already seen getters (Chapter 1) as the safe way to *read* a private field from outside the class. A **setter** (also called a *mutator method*) is the equivalent for *writing* — a public void method that sets the value of a private field. The key advantage over direct assignment: a setter can *validate* the new value before accepting it.

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

## 2. Validation in Setters

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

## 3. When NOT to Write a Setter

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

## 4. The Full Pattern

Now that you know both getters (Chapter 1) and setters, here's a complete class using both:

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

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `public void setAge(int newAge) { return newAge; }` | Setters are void — they assign, don't return | Remove `return newAge`; assign instead: `age = newAge;` |
| Setter with no validation | Works, but misses the point | Add an `if` to reject invalid values |

---

## Check Your Understanding

!!! information

    **Unit 4 · Chapter 2**

    Given:
    ```java
    public class Rectangle {
        private double width;
        private double height;

        public Rectangle(double width, double height) {
            this.width  = width;
            this.height = height;
        }

        public double getWidth()  { return width; }
        public double getHeight() { return height; }
    }
    ```

    **1.** Write a setter for `width` that rejects zero or negative values.

    **2.** Should `Rectangle` have a setter for `height`? Does it depend on anything?

    ---
    ---

    ## Answer Key

    **1.**
    ```java
    public void setWidth(double newWidth) {
        if (newWidth > 0) {
            width = newWidth;
        }
    }
    ```

    **2.** It depends on the design. If rectangles are supposed to be resizable, yes. If they're immutable (fixed at construction), no. The class designer makes this call.

---

## Homework 14: Setters

!!! attention

    **Unit 4 · Chapter 2**

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

    5. Predict the output.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Alex", 10, 3.2);
        s.setGpa(3.8);
        s.setGrade(11);
        System.out.println(s.toString());
    }
    ```

    6. Predict the output.
    ```java
    public static void main(String[] args) {
        Student s = new Student("Jordan", 11, 3.6);
        s.setGpa(-1.0);
        s.setGrade(13);
        System.out.println(s.getGpa());
        System.out.println(s.getGrade());
    }
    ```

    7. Predict the output.
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

    11. Find the bug.
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
