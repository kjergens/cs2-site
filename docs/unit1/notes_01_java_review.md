# CS2 — Unit 1 Notes 1: Java Review
These notes cover the CS1 concepts you are expected to know coming into CS2. Use them to refresh your memory, fill in gaps, or catch up if you missed a class. Every topic here will appear in homework, quizzes, and tests this semester.

---

## 1. Variables and Data Types

A **variable** stores a value in memory. Every variable has a **type** that tells Java what kind of value it holds.

| Type | Stores | Example |
|---|---|---|
| `int` | Whole numbers | `int age = 17;` |
| `double` | Decimal numbers | `double price = 4.99;` |
| `String` | Text (always in quotes) | `String name = "Alex";` |
| `boolean` | True or false only | `boolean done = false;` |

**Declaring vs. initializing:**
```java
int score;           // declared — exists but has no value yet
int score = 100;     // declared and initialized — has a value
```

**Types matter.** Java will not automatically convert between types in ways that lose information:
```java
int x = 3.7;         // ERROR — can't store a double in an int without casting
double y = 3;        // OK — Java converts 3 to 3.0 automatically
```

---

## 2. Printing

`System.out.println()` prints a value and moves to the next line.  
`System.out.print()` prints without moving to the next line.

```java
System.out.println("Hello");   // prints: Hello (then newline)
System.out.print("Hello");     // prints: Hello (stays on same line)
```

### String Concatenation

Use `+` to join a String with another value:
```java
String name = "Maya";
int age = 16;
System.out.println("Name: " + name);         // Name: Maya
System.out.println("Age: " + age);           // Age: 16
System.out.println("Name: " + name + ", Age: " + age);  // Name: Maya, Age: 16
```

### The Concatenation Trap

When `+` appears in a print statement, Java reads left to right. If it sees a String first, everything after is treated as concatenation — not math:
```java
int x = 10;
System.out.println("Value: " + x + 5);    // Value: 105  ← NOT what you might expect!
System.out.println("Value: " + (x + 5));  // Value: 15   ← parentheses force the math first
```

---

## 3. Arithmetic Operators

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `+` | Addition | `7 + 2` | `9` |
| `-` | Subtraction | `7 - 2` | `5` |
| `*` | Multiplication | `7 * 2` | `14` |
| `/` | Division | `7 / 2` | `3` ← see below |
| `%` | Remainder (modulo) | `7 % 2` | `1` |

### Integer Division

When both operands are `int`, Java divides and **drops the decimal** — it does not round:
```java
System.out.println(7 / 2);    // 3  (not 3.5)
System.out.println(9 / 4);    // 2  (not 2.25)
System.out.println(10 / 3);   // 3  (not 3.33)
```

To get a decimal result, at least one operand must be a `double`:
```java
System.out.println(7.0 / 2);          // 3.5
System.out.println((double) 7 / 2);   // 3.5  ← type casting
```

### The Modulo Operator `%`

`%` gives the **remainder** after division. It's useful for:

- Checking if a number is even: `n % 2 == 0`

- Checking divisibility: `n % 5 == 0`

- Cycling through a range

```java
System.out.println(7 % 2);   // 1  (7 = 3×2 + 1)
System.out.println(9 % 3);   // 0  (9 = 3×3 + 0)
System.out.println(15 % 4);  // 3  (15 = 3×4 + 3)
```

### Shorthand Assignment Operators

```java
int x = 10;
x += 3;   // same as: x = x + 3  →  x is now 13
x -= 2;   // same as: x = x - 2  →  x is now 11
x *= 2;   // same as: x = x * 2  →  x is now 22
x /= 4;   // same as: x = x / 4  →  x is now 5
x++;      // same as: x = x + 1  →  x is now 6
x--;      // same as: x = x - 1  →  x is now 5
```

---

## 4. Conditionals

An `if` statement runs a block of code only when a condition is true.

```java
if (condition) {
    // runs if condition is true
} else if (anotherCondition) {
    // runs if first condition is false and this one is true
} else {
    // runs if all conditions above are false
}
```

Java evaluates conditions top to bottom and **stops at the first true one**.

```java
int score = 82;
if (score >= 90) {
    System.out.println("A");
} else if (score >= 80) {    // checked only if score < 90
    System.out.println("B"); // prints this — score is 82
} else if (score >= 70) {    // skipped — already found a match
    System.out.println("C");
} else {
    System.out.println("Below C");
}
```

### Comparison Operators

| Operator | Meaning |
|---|---|
| `==` | Equal to |
| `!=` | Not equal to |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal to |
| `>=` | Greater than or equal to |

### Comparing Strings

**Never use `==` to compare Strings.** Use `.equals()`:
```java
String input = "hello";
if (input.equals("hello")) {     // correct
    System.out.println("Match!");
}
if (input == "hello") {          // WRONG — may not work as expected
    System.out.println("Match!");
}
```

Why? `==` checks if two variables point to the same object in memory. `.equals()` checks if the contents are the same. For Strings, you almost always want `.equals()`.

---

## 5. Boolean Logic

A `boolean` variable holds either `true` or `false`. You can combine conditions using logical operators:

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `&&` | AND — both must be true | `true && false` | `false` |
| `||` | OR — at least one must be true | `true || false` | `true` |
| `!` | NOT — flips the value | `!true` | `false` |

```java
boolean raining = true;
boolean cold = false;

System.out.println(raining && cold);    // false — both must be true
System.out.println(raining || cold);    // true  — raining is true, that's enough
System.out.println(!raining);           // false — flips true to false
System.out.println(!raining || cold);   // false — !raining is false, cold is false
```

### Using Booleans in Conditionals

```java
boolean sunny = true;
boolean weekend = true;

if (sunny && weekend) {
    System.out.println("Great day for a picnic!");
}
```

---

## Quick Reference: Common Mistakes

| Mistake | Problem | Fix |
|---|---|---|
| `int x = 7 / 2;` expecting 3.5 | Integer division drops decimal | Use `(double) 7 / 2` or `7.0 / 2` |
| `"score: " + x + 5` | Concatenates 5 as a String | Use `"score: " + (x + 5)` |
| `if (input == "hello")` | Compares object references | Use `input.equals("hello")` |
| `if (x = 5)` | Assignment, not comparison | Use `if (x == 5)` |
| Missing `}` on an else | Misplaced brace | `else` must come right after the closing `}` of the `if` |

---

## Check Your Understanding

!!! information

    **Unit 1 · Chapter 1**

    Use these questions to test yourself before a quiz. If you get something wrong, go back to the section in these notes that covers it.

    ---

    ### Part A: Concepts

    **1.** What is the difference between `=` and `==` in Java?

    **2.** What does the `%` operator return? Give an example of when it would be useful.

    **3.** Why is `input == "hello"` unreliable for comparing Strings? What should you use instead?

    **4.** What is type casting? Write one line of code that casts an `int` variable called `total` to a `double` before dividing it by `4`.

    **5.** What is the difference between `&&` and `||`? 

    **6.** Complete the table:

    | `a` | `b` | `a && b` | `a \|\| b` | `!a` |
    |---|---|---|---|---|
    | true | true | | | |
    | true | false | | | |
    | false | true | | | |
    | false | false | | | |

    **7.** Java evaluates `if / else if / else` chains from top to bottom and stops at the first true condition. Why does the order of conditions matter? Give an example where getting the order wrong would produce an incorrect result.

    **8.** Without running it, what does this print — and why?
    ```java
    System.out.println("Result: " + 4 + 5);
    System.out.println("Result: " + (4 + 5));
    ```

    ---

    ### Part B: Predict the Output

    **9.** Predict the output:
    ```java
    int x = 17;
    int y = 5;
    System.out.println(x / y);
    System.out.println(x % y);
    System.out.println((double) x / y);
    ```

    **10.** Predict the output:
    ```java
    boolean hungry = false;
    boolean tired = true;
    System.out.println(hungry || tired);
    System.out.println(hungry && tired);
    System.out.println(!tired);
    System.out.println(!hungry && tired);
    ```

    **11.** Predict the output:
    ```java
    int n = 12;
    if (n % 2 == 0 && n > 10) {
        System.out.println("Big even");
    } else if (n % 2 == 0) {
        System.out.println("Small even");
    } else {
        System.out.println("Odd");
    }
    ```

    **12.** Predict the output:
    ```java
    int a = 3;
    int b = 4;
    System.out.println("Sum: " + a + b);
    System.out.println("Sum: " + (a + b));
    System.out.println(a + b + " is the sum");
    ```

    *(Question 12 has a twist — think carefully about the last line.)*

    ---

    ### Part C: Write the Code

    **13.** A variable `int n` is already declared. Write code that prints:
    - `"even"` if `n` is divisible by 2
    - `"odd"` otherwise

    **14.** Write code that prints `"Divisible by both"` if a variable `int n` is divisible by both 3 and 7, and `"Not divisible by both"` otherwise.

    **15.** Write a complete if / else if / else block for a variable `int temp` (already declared) that prints:
    - `"Cold"` if temp is below 40
    - `"Comfortable"` if temp is between 40 and 79 (inclusive)
    - `"Hot"` if temp is 80 or above

    **16.** What is wrong with this swap attempt? Then write the correct version.
    ```java
    int x = 5;
    int y = 10;
    x = y;
    y = x;
    ```

    ---

    ### Part D: Find the Bug

    **17.** Find the bug:
    ```java
    int score = 95;
    if (score >= 60) {
        System.out.println("Passing");
    } else if (score >= 90) {
        System.out.println("Excellent");
    }
    ```
    This compiles and runs but produces the wrong output for a score of 95. What is the bug?

    **18.** Find the bug:
    ```java
    double total = 49.99;
    int rounded = total;
    System.out.println(rounded);
    ```

    **19.** Find the bug:
    ```java
    String answer = "yes";
    if (answer.equals = "yes") {
        System.out.println("Confirmed");
    }
    ```

    ---
    ---

    ## Answer Key

    ### Part A: Concepts

    **1.** `=` is the assignment operator — it stores a value into a variable (`int x = 5`). `==` is the equality operator — it compares two values and returns true or false (`if (x == 5)`). Using `=` inside an `if` condition is a compile error.

    **2.** `%` returns the remainder after integer division. Example: `17 % 5` returns `2` because 17 = 3×5 + 2. Useful for: checking if a number is even (`n % 2 == 0`), checking divisibility (`n % 3 == 0`), cycling through a fixed range.

    **3.** `==` checks whether two variables point to the same object in memory — not whether their contents are the same. Two String variables can hold identical text but be different objects, so `==` may return false even when the text matches. Use `.equals()` to compare the actual content.

    **4.** Type casting converts one type to another. Example:
    ```java
    System.out.println((double) total / 4);
    ```
    The `(double)` cast converts `total` to a double before the division, so the result is decimal.

    **5.** && is "AND" (both conditions need to be true for the whole epxression to be true). || is "OR" (as long as at least one condition is true the whole expression will resolve to true)

    **6.** Truth table:

    | `a` | `b` | `a && b` | `a \|\| b` | `!a` |
    |---|---|---|---|---|
    | true | true | true | true | false |
    | true | false | false | true | false |
    | false | true | false | true | true |
    | false | false | false | false | true |

    **7.** Because Java stops at the first true condition, a broader condition placed before a narrower one will "catch" cases that were meant to fall through. Example: if you check `score >= 60` before `score >= 90`, every score of 90 or above matches the first condition and prints `"Passing"` — it never reaches the `"Excellent"` branch.

    **8.**
    ```
    Result: 45
    Result: 9
    ```
    First line: Java sees a String first, so `4` and `5` are concatenated as text → `"45"`.  
    Second line: parentheses force `4 + 5 = 9` first, then it's appended to the String → `"9"`.

    ---

    ### Part B: Predict the Output

    **9.**
    ```
    3
    2
    3.4
    ```
    `17 / 5 = 3` (integer division drops remainder). `17 % 5 = 2` (remainder). `(double) 17 / 5 = 3.4`.

    **10.**
    ```
    true
    false
    false
    true
    ```
    `false || true = true`. `false && true = false`. `!true = false`. `!false && true = true && true = true`.

    **11.**
    ```
    Big even
    ```
    `12 % 2 == 0` is true AND `12 > 10` is true → first condition matches, prints `"Big even"` and stops.

    **12.**
    ```
    Sum: 34
    Sum: 7
    7 is the sum
    ```
    Line 1: String comes first, so `3` and `4` are concatenated → `"34"`.  
    Line 2: parentheses compute `3 + 4 = 7` first → `"7"`.  
    Line 3: `a + b` is evaluated first because there is no String yet on the left — it adds `3 + 4 = 7`, then concatenates `" is the sum"` → `"7 is the sum"`.

    ---

    ### Part C: Write the Code

    **13.**
    ```java
    if (n % 2 == 0) {
        System.out.println("even");
    } else {
        System.out.println("odd");
    }
    ```

    **14.**
    ```java
    if (n % 3 == 0 && n % 7 == 0) {
        System.out.println("Divisible by both");
    } else {
        System.out.println("Not divisible by both");
    }
    ```

    **15.**
    ```java
    if (temp < 40) {
        System.out.println("Cold");
    } else if (temp <= 79) {
        System.out.println("Comfortable");
    } else {
        System.out.println("Hot");
    }
    ```

    **16.** The bug: after `x = y`, x holds 10 — but then `y = x` sets y to 10 as well. The original value of x (5) is lost. You need a temporary variable:
    ```java
    int temp = x;
    x = y;
    y = temp;
    ```

    ---

    ### Part D: Find the Bug

    **17.** The conditions are in the wrong order. `score >= 60` is checked first — a score of 95 satisfies it immediately and prints `"Passing"`. The `"Excellent"` branch is never reached. Fix: check the more restrictive condition first:
    ```java
    if (score >= 90) {
        System.out.println("Excellent");
    } else if (score >= 60) {
        System.out.println("Passing");
    }
    ```

    **18.** You cannot assign a `double` directly to an `int` — Java will not do this automatically because information (the decimal) would be lost. Fix: use a cast or change the type:
    ```java
    int rounded = (int) total;   // truncates to 49
    ```

    **19.** `.equals` is a method and must be called with parentheses and a dot. The `= "yes"` syntax is not valid. Fix:
    ```java
    if (answer.equals("yes")) {
        System.out.println("Confirmed");
    }
    ```

---

## Homework 1: Java Review

!!! attention

    **Unit 1 · Chapter 1**

    *Assigned Class 1 · Due Class 2*

    **Rubric (100 points):** Part 1 (Q1–5) 20 pts · Part 2 (Q6–9) 60 pts · Part 3 (Q10–13) 20 pts.

    ### Part 1: Predict the Output

    Write exactly what each program prints. If a line produces no output, skip it.

    1.
    ```java
    int a = 7;
    int b = 2;
    System.out.println(a / b);
    System.out.println(a % b);
    System.out.println((double) a / b);
    ```

    2.
    ```java
    int x = 10;
    String label = "score";
    System.out.println("Your " + label + ": " + x);
    System.out.println("Your " + label + ": " + (x + 5));
    System.out.println("Your " + label + ": " + x + 5);
    ```

    3.
    ```java
    int score = 82;
    if (score >= 90) {
        System.out.println("A");
    } else if (score >= 80) {
        System.out.println("B");
    } else if (score >= 70) {
        System.out.println("C");
    } else {
        System.out.println("Below C");
    }
    ```

    4.
    ```java
    int n = 15;
    if (n % 3 == 0 && n % 5 == 0) {
        System.out.println("FizzBuzz");
    } else if (n % 3 == 0) {
        System.out.println("Fizz");
    } else if (n % 5 == 0) {
        System.out.println("Buzz");
    } else {
        System.out.println(n);
    }
    ```

    5.
    ```java
    boolean raining = true;
    boolean cold = false;
    System.out.println(raining && cold);
    System.out.println(raining || cold);
    System.out.println(!raining);
    System.out.println(!raining || cold);
    ```

    ### Part 2: Write the Code

    6. Write a complete if/else if/else block that prints a letter grade based on an `int score` (already declared): 90+ → `"A"`, 80–89 → `"B"`, 70–79 → `"C"`, 60–69 → `"D"`, below 60 → `"F"`.

    7. Write code to swap the values of two `int` variables `x` and `y` without changing either variable's name. After your code, `x` should hold the original value of `y` and vice versa. (Hint: you need a third variable.)

    8. A variable `int temperature` is already declared. Write a single `if` statement (no `else`) that prints `"Stay inside"` only if the temperature is below 20 or above 95.

    9. Write a complete Java program that declares a `String name` and an `int age` (any values), then uses if/else to print `"Welcome, [name]!"` if age is 13 or older, or `"Sorry, you must be at least 13."` otherwise.

    ### Part 3: Find the Bug

    Each snippet has exactly one error. Identify the line and describe the problem.

    10.
    ```java
    String password = "java123";
    String input = "java123";
    if (input == password) {
        System.out.println("Access granted");
    } else {
        System.out.println("Wrong password");
    }
    ```

    11.
    ```java
    int x = 5;
    if (x = 5) {
        System.out.println("x is five");
    }
    ```

    12.
    ```java
    int age = 20;
    if (age >= 18) {
        System.out.println("Adult");
        else {
        System.out.println("Minor");
    }
    ```

    13.
    ```java
    double price = 9.99;
    int quantity = 3;
    double total = price * quantity;
    System.out.println("Total: $" + total);

    int discount = total * 0.1;
    System.out.println("Discount: $" + discount);
    ```
