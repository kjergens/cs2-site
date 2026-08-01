# CS2 — Unit Notes: Method Overloading

---

## 1. What Is Overloading?

**Method overloading** means writing multiple methods with the same name but different **parameter lists**. Java decides which version to call based on the number and types of the arguments at the call site — at compile time, before the program runs.

```java
public static int max(int a, int b) {
    return (a > b) ? a : b;
}

public static int max(int a, int b, int c) {
    return max(max(a, b), c);
}
```

Both methods are named `max`. Java chooses:
- `max(3, 7)` → calls the two-parameter version → `7`
- `max(3, 7, 5)` → calls the three-parameter version → `7`

---

## 2. What Makes a Valid Overload?

A valid overload must differ in the **parameter list** — the number of parameters, their types, or both.

| Valid? | Why |
|---|---|
| `add(int a, int b)` vs `add(double a, double b)` | ✓ Different parameter types |
| `add(int a, int b)` vs `add(int a, int b, int c)` | ✓ Different number of parameters |
| `print(String s)` vs `print(int n)` | ✓ Different parameter type |
| `add(int a, int b)` vs `add(int x, int y)` | ✗ Same types, same count — just renamed |
| `int add(int a, int b)` vs `double add(int a, int b)` | ✗ Return type alone is not enough |

**The rule:** overloads must differ in parameter count or parameter types. Return type does NOT distinguish overloads.

---

## 3. Which Version Gets Called?

Java matches the call to the method whose parameter types best fit the argument types.

```java
public static String describe(int n)    { return "integer: " + n; }
public static String describe(double d) { return "double: " + d; }
public static String describe(String s) { return "string: " + s; }
```

| Call | Match | Returns |
|---|---|---|
| `describe(42)` | `describe(int)` | `"integer: 42"` |
| `describe(3.14)` | `describe(double)` | `"double: 3.14"` |
| `describe("hi")` | `describe(String)` | `"string: hi"` |
| `describe(5 / 2)` | `describe(int)` — `5/2` is `2`, an int | `"integer: 2"` |

Watch out: `5 / 2` is integer division, so the argument is `2` (int), not `2.5` (double).

---

## 4. Calling One Overload from Another

The three-parameter max doesn't need its own if-statement logic — it can call the two-parameter version twice:

```java
public static int max(int a, int b) {
    return (a > b) ? a : b;
}

public static int max(int a, int b, int c) {
    return max(max(a, b), c);   // calls the 2-param version twice
}
```

This is decomposition: build complex behavior out of simpler pieces you've already written.

---

## 5. Overloaded area Methods

```java
public static double area(double side) {
    return side * side;                          // square
}

public static double area(double width, double height) {
    return width * height;                       // rectangle
}

public static double area(double base, double height, boolean isTriangle) {
    return 0.5 * base * height;                  // triangle
}
```

The `boolean isTriangle` parameter in the third version exists only to make the signature distinct from the rectangle version (which also has two doubles). Without it, `area(3.0, 4.0)` would be ambiguous.

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| Same parameter types and count, different names only | Not a valid overload — compile error (duplicate method) | Change parameter types or count |
| Same parameter types and count, different return type only | Not a valid overload — compile error | Change parameter types or count |
| `describe(5 / 2)` expected to call `describe(double)` | `5/2` is int division — result is `2` (int), calls `describe(int)` | Use `describe(5.0 / 2)` or `describe(2.5)` |

---

## Check Your Understanding

### Part A: Valid or Not?

For each pair, state whether it's a valid overload and why.

**1.**
```java
public static int add(int a, int b)
public static int add(int x, int y)
```

**2.**
```java
public static int add(int a, int b)
public static double add(double a, double b)
```

**3.**
```java
public static int multiply(int a, int b)
public static int multiply(int a, int b, int c)
```

---

### Part B: Which Version Gets Called?

Given:
```java
public static String describe(int n)    { return "int: " + n; }
public static String describe(double d) { return "double: " + d; }
public static String describe(String s) { return "string: " + s; }
```

**4.** `describe(10)`  
**5.** `describe(10.0)`  
**6.** `describe("hello")`  
**7.** `describe(5 / 2)` — what does this print and why?

---
---

## Answer Key

### Part A

**1.** Not valid. Same parameter types and count — only the names differ. Names don't matter for overloading.

**2.** Valid. Parameter types differ (`int` vs `double`).

**3.** Valid. Parameter count differs (2 vs 3).

### Part B

**4.** `"int: 10"` — argument is an int literal.

**5.** `"double: 10.0"` — argument is a double literal.

**6.** `"string: hello"` — argument is a String.

**7.** `"int: 2"` — `5 / 2` is integer division, result is `2` (int), so `describe(int)` is called.

---

## Homework 9: Method Overloading

*Assigned Class 10 · Due Class 11*

### Part 1: Valid or Not?

For each pair of method headers, state whether they form a valid overload. Explain your answer in one sentence.

1.
```java
public static int add(int a, int b)
public static int add(int x, int y)
```
2.
```java
public static int add(int a, int b)
public static double add(double a, double b)
```
3.
```java
public static void print(String s)
public static void print(int n)
public static void print(double d)
```
4.
```java
public static int multiply(int a, int b)
public static int multiply(int a, int b, int c)
```
5.
```java
public static double area(double radius)
public static double area(double width, double height)
```

### Part 2: Which Version Gets Called?

Given these overloaded methods:
```java
public static String describe(int n) {
    return "integer: " + n;
}
public static String describe(double d) {
    return "double: " + d;
}
public static String describe(String s) {
    return "string: " + s;
}
public static String describe(int a, int b) {
    return "two ints: " + a + " and " + b;
}
```

Write the method header that matches each call, then write what the call returns.

6. `describe(42)`
7. `describe(3.14)`
8. `describe("hello")`
9. `describe(1, 2)`
10. What does this `main` print?
```java
public static void main(String[] args) {
    System.out.println(describe(10));
    System.out.println(describe(10.0));
    System.out.println(describe(describe(5)));
}
```

### Part 3: Write Overloaded Methods

11. Write an overloaded pair of methods named `max`:
   - `max(int a, int b)` — returns the larger of two integers
   - `max(int a, int b, int c)` — returns the largest of three integers, by calling the two-parameter version twice (do not use if statements in the three-parameter version)

12. Write three overloaded versions of a method named `area` that each return a `double`:
   - `area(double side)` — area of a square (side²)
   - `area(double width, double height)` — area of a rectangle
   - `area(double base, double height, boolean isTriangle)` — area of a triangle (½ × base × height); the boolean parameter exists only to make the signature distinct from the rectangle version

### Part 4: Find the Bug

13.
```java
public static int total(int a, int b) {
    return a + b;
}
public static int total(int x, int y) {
    return x * y;
}
```

14.
```java
public static double round2(double n) {
    return Math.round(n * 100.0) / 100.0;
}
public static float round2(double n) {
    return (float)(Math.round(n * 100.0) / 100.0);
}
```

15. This code compiles and runs but the programmer is confused about the output. What is happening?
```java
public static String classify(int n) {
    return "int";
}
public static String classify(double d) {
    return "double";
}

public static void main(String[] args) {
    System.out.println(classify(5));        // expected "int"
    System.out.println(classify(5.0));      // expected "double"
    System.out.println(classify(5 / 2));    // expected "double" — surprised to see "int"
}
```
