# CS2 — Unit Notes: Encapsulation

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

## 5. The Encapsulated BankAccount

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

`getBalance()` is the only way for outside code to read the balance. There is no `setBalance()` — the balance only changes through `deposit` and `withdraw`, which enforce the rules.

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `acct.balance = 100` from outside the class | Compile error — field is private | Use a method: `acct.deposit(100)` |
| `acct.balance` in a print statement from outside | Compile error — field is private | Call `acct.getBalance()` instead |
| Forgetting `private` on some fields | Inconsistent encapsulation — some fields accessible, others not | Make all instance variables `private` |

---

## Check Your Understanding

**1.** What does `private` mean for an instance variable?

**2.** Why is it better to make instance variables `private` rather than leaving them `public`?

**3.** Given a `private double balance` field, which of the following will compile if written *outside* the class?
- `acct.balance = 100.0`
- `acct.deposit(100.0)`
- `System.out.println(acct.balance)`
- `System.out.println(acct.getBalance())`

**4.** Can a method inside `BankAccount` access `private double balance`? Why or why not?

---
---

## Answer Key

**1.** `private` means only code inside the same class can access it. Code outside the class gets a compile error if it tries to read or write the field directly.

**2.** Private fields let the class control its own state. Methods can validate values before changing them; direct access allows any code to put the object into an invalid state.

**3.** Compile error: `acct.balance = 100.0` and `System.out.println(acct.balance)`. Compiles: `acct.deposit(100.0)` and `acct.getBalance()`.

**4.** Yes. The `private` restriction applies to code *outside* the class. Methods inside `BankAccount` can access `balance` freely.
