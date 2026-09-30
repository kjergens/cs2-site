# CS2 — Unit 2 Chapter 1: Methods — Input, Output

A **method** is a named block of code that performs a specific task. You define it once and call it as many times as needed.

---

## 1. Why Methods?

**Problem without methods:** If you need to repeat the same logic in three places, you copy it three times. When you want to change it, you edit three places and hope you don't miss one.

```java
// Without a method — three copies of the same calculation
System.out.println(5 * 5);
System.out.println(9 * 9);
System.out.println(12 * 12);
```

**Solution with a method:**

```java
public static void main(String[] args) {
    System.out.println(square(5));
    System.out.println(square(9));
    System.out.println(square(12));
}

public static int square(int n) {
    return n * n;
}
```

Now the squaring logic exists in one place. `main` reads like an outline of what the program does.

**Three benefits:**
- **DRY** (Don't Repeat Yourself) — write the logic once
- **Decomposition** — break a big problem into named smaller pieces
- **Readability** — `main` reads like an outline of what the program does

---

## 2. Methods Are Input → Output

Think of a method the way you think of a math function. `f(x) = x²` takes an input and produces an output — you don't ask `f` to *print* anything, you ask it for a value, and then *you* decide what to do with that value.

```java
public static int square(int n) {
    return n * n;
}
```

| Part | Example | Meaning |
|---|---|---|
| Access modifier | `public` | Visible to the whole program |
| `static` | `static` | Belongs to the class, not an object |
| **Return type** | `int` | The type of value this method hands back — the *output* |
| Method name | `square` | What you call it |
| **Parameter list** | `int n` | The *input(s)* the method receives |
| Body | `{ return n * n; }` | The code that computes the output |

**Calling it:**
```java
System.out.println(square(5));   // 25
int result = square(9);          // 81, stored in a variable
```

The value you pass in (`5`, `9`) is called an **argument**. The variable that receives it inside the method (`n`) is called a **parameter**.

---

## 3. The `return` Statement

The `return` statement does two things:
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

**Every path through the method must return a value.** If Java can reach the end of a non-`void` method without hitting a `return`, it is a compile error.

The return type — `int`, `double`, `boolean`, `String`, etc. — tells Java what type of value is coming back. `void` (covered next chapter) is the one special case: it means *nothing* comes back.

---

## 4. Multiple Parameters

A method can take multiple parameters, separated by commas. Each has its own type and name — think of them as the multiple inputs to a multi-variable function, like `f(x, y)`.

```java
public static int add(int a, int b) {
    return a + b;
}
```

**Calling it:**
```java
add(3, 4);        // 7
add(add(1, 2), add(3, 4));   // add(3, 7) → 10
```

**Order matters:** the first argument maps to the first parameter, the second to the second. Types must match.

---

## 5. Parameters Are Copies (Passed by Value)

When you pass a **primitive** (`int`, `double`, `boolean`, etc.) to a method, Java gives the method its own **copy**. Changing the copy inside the method has no effect on the original variable in the caller.

```java
public static void main(String[] args) {
    int x = 5;
    int y = triple(x);
    System.out.println(x);   // 5 — unchanged
    System.out.println(y);   // 15 — the returned value
}

public static int triple(int n) {
    n = n * 3;
    return n;
}
```

`x` never changes. `triple` only ever modified its own copy, `n`; the *only* way information gets back to the caller is through `return`.

**Local scope:** a variable declared inside a method — including its parameters — only exists inside that method. It's created when the method is called and destroyed when the method returns.

```java
public static void main(String[] args) {
    int r = 0;                   // local to main() — only exists in main
    int x = compute(4);
    System.out.println(x);       // never runs — compute() below fails to compile
}

public static int compute(int a) {
    a = a + r;   // COMPILE ERROR — r is local to main(), not visible here
    return a;
}
```

**This is not about which method is written first in the file** — `r` is declared *above* `compute` in the file, and `compute` still can't see it. Java doesn't care about definition order; the error is about the `{ }` boundaries. `r` only exists between `main`'s own braces, no matter what's written above or below it.

Two different methods can each declare a variable with the same name (`count`, `result`, whatever) with no conflict — each one only lives while its own method is running.

---

## 6. Using Return Values

A returned value isn't printed automatically — the caller decides what to do with it:

```java
public static void main(String[] args) {
    System.out.println(square(4));               // print it directly: 16
    int x = square(5);                            // store it: 25
    System.out.println(square(3) + square(4));   // use it in an expression: 25
    System.out.println(square(square(2)));       // pass it into another call: 16
}

public static int square(int n) {
    return n * n;
}
```

**Rule going forward:** methods should *compute and return*. Don't print inside a method that's meant to produce a value — hand the value back and let the caller decide whether to print it, store it, or use it in more math.

---

## 7. Boolean-Returning Methods

A method that returns `boolean` can be used directly in an `if` condition — it's still just a value being handed back, the value just happens to be `true` or `false`.

```java
public static void main(String[] args) {
    System.out.println(isEven(6));   // true
    if (isEven(10)) {
        System.out.println("ten is even");
    }
}

public static boolean isEven(int n) {
    return n % 2 == 0;
}
```

---

## 8. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `public static int tripleIt(int n) { int result = n * 3; }` | Missing `return` — compile error | Add `return result;` |
| `public static double half(int n) { return n / 2; }` | Integer division — returns wrong answer | Cast: `return (double) n / 2;` |
| `public static boolean isNeg(int n) { if (n < 0) { return true; } }` | Not all paths return — compile error | Add `return false;` after the `if` |
| `square(4, 5)` when method only takes one param | Wrong number of arguments | Match the parameter count |
| `square("four")` | Wrong argument type | Pass an `int`, not a `String` |
| `System.out.println(result)` outside the method that declared `result` | Compile error: variable not in scope | Variables are local — they can't escape their method |
| Assuming a method must be written above where it's called | Doesn't apply in Java — method order within the class doesn't matter | Methods can be defined in any order; only *variable* scope (the `{ }` a variable is declared in) matters |

---

## Check Your Understanding

!!! information

    **Unit 2 · Chapter 1: Methods — Input, Output**

    Test your understanding of methods with the questions and exercises below. Then check your work with the Answer Key.

    1. Name three benefits of using methods instead of copying code.
    - A method's header is `public static double circleArea(double radius)`. What is the input? What is the output?
    - Predict the output.
    ```java
    public static void main(String[] args) {
        int result = add(add(1, 2), add(3, 4));
        System.out.println(result);
    }
    public static int add(int a, int b) { return a + b; }
    ```
    - Predict the output, then explain why `x` prints the value it does.
    ```java
    public static void main(String[] args) {
        int x = 5;
        int y = triple(x);
        System.out.println(x);
        System.out.println(y);
    }
    public static int triple(int n) { return n * 3; }
    ```
    - Find the bug.
    ```java
    public static boolean isNegative(int n) {
        if (n < 0) {
            return true;
        }
    }
    ```
    - Write a method `average` that takes two `double` parameters and returns their average.

    ---

    **Answer Key**

    1. DRY (write once, reuse), decomposition (break big problems into named pieces), readability (`main` reads like an outline).
    - Input: `radius` (a `double`). Output: the circle's area (a `double`), handed back via `return`.
    - `10` — `add(1,2)` = 3, `add(3,4)` = 7, `add(3,7)` = 10.
    - Output:
    ```
    5
    15
    ```
    `x` is unchanged because `triple` only modifies its own copy of the parameter, `n`. The only value that reaches `main` is the one `triple` returns, stored in `y`.
    - Not all paths return a value — if `n >= 0`, the method ends without returning anything. Add `return false;` after the `if` block.

    -
    ```java
    public static double average(double a, double b) {
        return (a + b) / 2;
    }
    ```

---

## Homework 5: Methods — Input, Output

!!! attention

    **Unit 2 · Chapter 1 Homework**

    **Rule going forward:** methods should compute and return; `main` (or the caller) decides what to do with the result — including whether to print it.

    ### Part 1: Why Methods?

    1. If you needed to change how `square` computes its result, how many lines would you edit if the calculation appeared inline in three places in `main`? How many if it lived inside a `square` method?
    2. What is the programming term for breaking a big problem into small, named pieces like separate methods?

    ### Part 2: Reading Method Headers

    For each method header, state the input(s) and the output.

    3. `public static int countVowels(String s)`
    4. `public static double circleArea(double radius)`
    5. `public static boolean isPrime(int n)`
    6. `public static String initials(String first, String last)`

    ### Part 3: Predict the Output

    7. Predict the output.
    ```java
    public static void main(String[] args) {
        System.out.println(square(4));
        System.out.println(square(3) + square(4));
        int x = square(5);
        System.out.println(x);
    }

    public static int square(int n) {
        return n * n;
    }
    ```

    8. Predict the output.
    ```java
    public static void main(String[] args) {
        System.out.println(bigger(3, 7));
        System.out.println(bigger(bigger(2, 5), bigger(8, 4)));
    }

    public static int bigger(int a, int b) {
        if (a > b) {
            return a;
        }
        return b;
    }
    ```

    9. Trace through this code. What does `main` print for `x`, and what does it print for `y`?
    ```java
    public static void main(String[] args) {
        int x = 10;
        int y = addFive(x);
        System.out.println(x);
        System.out.println(y);
    }

    public static int addFive(int n) {
        n = n + 5;
        return n;
    }
    ```

    ### Part 4: Find the Bug

    10. Find the bug.
    ```java
    public static int tripleIt(int n) {
        int result = n * 3;
    }
    ```

    11. Compiles and runs — but returns the wrong answer. Why?
    ```java
    public static double half(int n) {
        return n / 2;
    }
    ```

    12. Find the bug.
    ```java
    public static int absolute(int n) {
        if (n < 0) {
            return -n;
        } else {
            return "positive";
        }
    }
    ```

    ### Part 5: Write the Method

    For each problem, write a method that returns the result — do not print inside the method.

    13. Write a method `celsiusToFahrenheit` that takes a `double` Celsius temperature and returns the Fahrenheit equivalent. Formula: `F = C × 9.0 / 5.0 + 32`.

    14. Write a method `clamp` that takes three `int` parameters — a value, a min, and a max — and returns the value if it falls within `[min, max]`, the min if the value is too low, or the max if it is too high. Examples: `clamp(5, 0, 10)` → `5`, `clamp(-3, 0, 10)` → `0`, `clamp(15, 0, 10)` → `10`.

    15. Write a method `hypotenuse` that takes two `double` parameters representing the legs of a right triangle and returns the length of the hypotenuse. Use `Math.sqrt( )` to get the square root.
