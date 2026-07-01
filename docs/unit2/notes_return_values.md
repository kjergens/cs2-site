# CS2 — Unit Notes: Return Values

---

## 1. Why Return Values?

A void method prints a result. But what if you need to *use* the result — store it, add it to something, pass it to another method?

```java
// Void version — prints, but you can't use the result
public static void squareAndPrint(int n) {
    System.out.println(n * n);
}

// Return version — gives the value back so you can use it
public static int square(int n) {
    return n * n;
}
```

With `square`, you can do:
```java
int result = square(5);           // store it
System.out.println(square(4));    // print it
System.out.println(square(3) + square(4));  // use it in math
```

**Rule going forward:** methods should *compute and return*. The caller decides what to do with the result — including whether to print it.

---

## 2. The Return Type

The return type appears where `void` used to be. It tells Java what type of value the method will send back.

```java
public static int square(int n) {
    return n * n;
}
```

| Return type | Means |
|---|---|
| `void` | Nothing comes back |
| `int` | An integer comes back |
| `double` | A decimal comes back |
| `boolean` | true or false comes back |
| `String` | A String comes back |

---

## 3. The return Statement

The `return` statement:
1. Sends the value back to the caller
2. Immediately ends the method — no code after `return` runs

```java
public static int bigger(int a, int b) {
    if (a > b) {
        return a;    // method ends here if a > b
    }
    return b;        // only reached if a <= b
}
```

**Every path through the method must return a value.** If Java can reach the end of a non-void method without hitting a `return`, it is a compile error.

---

## 4. Using Return Values

```java
public static int square(int n) {
    return n * n;
}

public static void main(String[] args) {
    System.out.println(square(4));              // 16
    System.out.println(square(3) + square(4)); // 25
    int x = square(5);
    System.out.println(x);                     // 25
}
```

Return values can be:
- Stored in a variable: `int x = square(5);`
- Printed directly: `System.out.println(square(5));`
- Used in expressions: `square(3) + square(4)`
- Passed as arguments: `square(square(2))` → `square(4)` → `16`

---

## 5. Boolean-Returning Methods

A method that returns `boolean` can be used directly in an `if` condition.

```java
public static boolean isEven(int n) {
    return n % 2 == 0;
}

public static void main(String[] args) {
    System.out.println(isEven(6));   // true
    System.out.println(isEven(7));   // false
    if (isEven(10)) {
        System.out.println("ten is even");
    }
}
```

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `public static int tripleIt(int n) { int result = n * 3; }` | Missing `return` — compile error | Add `return result;` |
| `public static double half(int n) { return n / 2; }` | Integer division — returns wrong answer | Cast: `return (double) n / 2;` |
| `public static boolean isNeg(int n) { if (n < 0) { return true; } }` | Not all paths return — compile error | Add `return false;` after the if |
| `int x = printStars(4);` | Void method used as if it returned a value | Void methods produce no value to store |

---

## Check Your Understanding

### Part A: Predict the Output

**1.**
```java
public static int square(int n) { return n * n; }
public static void main(String[] args) {
    System.out.println(square(4));
    System.out.println(square(3) + square(4));
    int x = square(5);
    System.out.println(x);
}
```

**2.**
```java
public static int add(int a, int b) { return a + b; }
public static void main(String[] args) {
    int result = add(add(1, 2), add(3, 4));
    System.out.println(result);
}
```

**3.**
```java
public static boolean isEven(int n) { return n % 2 == 0; }
public static void main(String[] args) {
    System.out.println(isEven(6));
    System.out.println(isEven(7));
    if (isEven(10)) { System.out.println("ten is even"); }
}
```

---

### Part B: Find the Bug

**4.**
```java
public static int tripleIt(int n) {
    int result = n * 3;
}
```

**5.**
```java
public static double half(int n) {
    return n / 2;
}
```

**6.**
```java
public static boolean isNegative(int n) {
    if (n < 0) {
        return true;
    }
}
```

---

### Part C: Write the Method

**7.** Write `celsiusToFahrenheit(double c)` — returns the Fahrenheit equivalent. Formula: F = C × 9.0 / 5.0 + 32.

**8.** Write `hypotenuse(double a, double b)` — returns the hypotenuse of a right triangle. Use `Math.sqrt` and `Math.pow`.

---
---

## Answer Key

### Part A

**1.**
```
16
25
25
```

**2.** `10` — `add(1,2)` = 3, `add(3,4)` = 7, `add(3,7)` = 10.

**3.**
```
true
false
ten is even
```

### Part B

**4.** Missing `return` — computes `result` but never sends it back. Add `return result;`.

**5.** Integer division: `n / 2` divides two ints and drops the decimal. Fix: `return (double) n / 2;`.

**6.** Not all paths return a value — if `n >= 0`, the method ends without returning anything. Add `return false;` after the if block.

### Part C

**7.**
```java
public static double celsiusToFahrenheit(double c) {
    return c * 9.0 / 5.0 + 32;
}
```

**8.**
```java
public static double hypotenuse(double a, double b) {
    return Math.sqrt(Math.pow(a, 2) + Math.pow(b, 2));
}
```
