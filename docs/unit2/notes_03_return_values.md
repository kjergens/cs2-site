# CS2 — Unit 2 Chapter 3: Choosing Between void and a Return Value

You now know both kinds of methods. This chapter is practice deciding which one fits a given problem, and moving comfortably between them.

---

## 1. The Decision

| Ask yourself... | Choose |
|---|---|
| Does the caller need to *use* this result — store it, do more math with it, pass it somewhere else? | Return a value |
| Is the method's whole job just to perform an action, most often printing? | `void` |
| Am I tempted to print *inside* a method that's also supposed to hand back a value? | Don't. Return the value; let the caller print it. |

The same underlying computation can almost always be written either way — the difference is who ends up in control of the output.

```java
// Version 1: void — the method controls the output
public static void printSquare(int n) {
    System.out.println(n * n);
}

// Version 2: return — the caller controls the output
public static int square(int n) {
    return n * n;
}
```

`printSquare` can only ever print. `square` can be printed, stored, added to something else, or passed into another method — `printSquare(5) + printSquare(4)` isn't even legal Java, but `square(5) + square(4)` is. **When in doubt, prefer a return value — it's more flexible, since a void result can't be reused.**

---

## 2. Refactoring Practice

**From return → void:** drop the `return`, replace it with the action.

```java
public static double average(double a, double b) {
    return (a + b) / 2;
}
```
becomes
```java
public static void printAverage(double a, double b) {
    System.out.println((a + b) / 2);
}
```

**From void → return:** replace the action with `return`, and give the method a real return type.

```java
public static void printIsEven(int n) {
    System.out.println(n % 2 == 0);
}
```
becomes
```java
public static boolean isEven(int n) {
    return n % 2 == 0;
}
```

---

## Check Your Understanding

!!! information

    **Unit 2 · Chapter 3**

    1. Rewrite this `void` method so it returns a value instead of printing:
    ```java
    public static void printMax(int a, int b) {
        if (a > b) {
            System.out.println(a);
        } else {
            System.out.println(b);
        }
    }
    ```
    2. Rewrite this returning method as a `void` method that prints its own result:
    ```java
    public static String greeting(String name) {
        return "Hello, " + name + "!";
    }
    ```
    3. A method needs to compute a shipping cost and then *also* add it to a running total elsewhere in the program. Should it be `void` or return a value? Why?
    4. A method just needs to print a divider line of dashes. Should it be `void` or return a value? Why?

    ---

    **Answer Key**

    1.
    ```java
    public static int max(int a, int b) {
        if (a > b) {
            return a;
        }
        return b;
    }
    ```
    2.
    ```java
    public static void printGreeting(String name) {
        System.out.println("Hello, " + name + "!");
    }
    ```
    3. Return a value — the result needs to be reused (added to a total), and a `void` method has nothing to hand off.
    4. `void` — there's no result to use elsewhere, only an action to perform.

---

## Homework 7: void vs. Return — Practice

!!! attention

    **Unit 2 · Chapter 3 Homework**

    ### Part 1: Refactor

    1. Rewrite as a method that returns a value instead of printing:
    ```java
    public static void printTripled(int n) {
        System.out.println(n * 3);
    }
    ```
    2. Rewrite as a `void` method that prints its own result:
    ```java
    public static boolean isPositive(int n) {
        return n > 0;
    }
    ```

    ### Part 2: Decide, Then Write

    For each, decide `void` or return-a-value, then write the method.

    3. `celsiusToFahrenheit(double c)` — converts and hands back the Fahrenheit equivalent. Formula: `F = C × 9.0 / 5.0 + 32`.
    4. `printReceipt(String item, double price)` — prints a formatted line like `Item: <item> — $<price>`.
    5. `clamp(int value, int min, int max)` — returns `value` if it's within `[min, max]`, otherwise the nearest bound. Examples: `clamp(5, 0, 10)` → `5`, `clamp(-3, 0, 10)` → `0`, `clamp(15, 0, 10)` → `10`.
    6. `hypotenuse(double a, double b)` — returns the hypotenuse of a right triangle with legs `a` and `b`. Use `Math.sqrt` and `Math.pow`.

    ### Part 3: Find the Bug

    7. Find the bug.
    ```java
    public static int absolute(int n) {
        if (n < 0) {
            return -n;
        } else {
            return "positive";
        }
    }
    ```
    8. Compiles and runs — but returns the wrong answer. Why?
    ```java
    public static double half(int n) {
        return n / 2;
    }
    ```
