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
