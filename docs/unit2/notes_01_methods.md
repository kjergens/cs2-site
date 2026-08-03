# CS2 — Unit 2 Chapter 1: Introduction to Methods

A **method** is a named block of code that performs a specific task. You define it once and call it as many times as needed.

---

## 1. Why Methods?

**Problem without methods:** If you need to repeat the same logic in three places, you copy it three times. When you want to change it, you edit three places and hope you don't miss one.

```java
// Without a method — three copies of the same banner
System.out.println("==========");
System.out.println("Welcome!");
System.out.println("==========");
System.out.println("Round 1 starting...");
System.out.println("==========");
System.out.println("Welcome!");
System.out.println("==========");
System.out.println("Round 2 starting...");
```

**Solution with a method:**

```java
public static void printBanner() {
    System.out.println("==========");
    System.out.println("Welcome!");
    System.out.println("==========");
}

public static void main(String[] args) {
    printBanner();
    System.out.println("Round 1 starting...");
    printBanner();
    System.out.println("Round 2 starting...");
}
```

Now "Welcome!" appears in one place. Changing it means editing one line.

**Three benefits:**
- **DRY** (Don't Repeat Yourself) — write the logic once
- **Decomposition** — break a big problem into named smaller pieces
- **Readability** — `main` reads like an outline of what the program does

---

## 2. Anatomy of a Method

```java
public static void printStars(int n) {
    for (int i = 0; i < n; i++) {
        System.out.print("* ");
    }
    System.out.println();
}
```

| Part | Example | Meaning |
|---|---|---|
| Access modifier | `public` | Visible to the whole program |
| `static` | `static` | Belongs to the class, not an object |
| Return type | `void` | Returns nothing |
| Method name | `printStars` | What you call it |
| Parameter list | `int n` | Input(s) the method receives |
| Body | `{ ... }` | The code that runs |

---

## 3. Void Methods

A **void** method performs an action but does not send a value back to the caller. The word `void` where the return type goes means "nothing comes back."

```java
public static void greet(String name) {
    System.out.println("Hello, " + name + "!");
}
```

**Calling it:**
```java
greet("Alice");   // prints: Hello, Alice!
greet("Bob");     // prints: Hello, Bob!
```

The method is called by name with parentheses. If it has parameters, you pass values (called **arguments**) inside the parentheses.

---

## 4. Parameters

Parameters are the inputs a method expects. They are declared in the method header and work like local variables inside the method body.

```java
public static void printMultiples(int n, int count) {
    for (int i = 1; i <= count; i++) {
        System.out.println(n * i);
    }
}
```

**Calling it:**
```java
printMultiples(3, 4);  // prints 3, 6, 9, 12
printMultiples(5, 3);  // prints 5, 10, 15
```

The values you pass (`3, 4`) are called **arguments**. They are assigned to the parameters (`n, count`) when the method is called.

**Order matters:** the first argument goes to the first parameter, the second to the second, and so on. Types must match.

---

## 5. How Method Calls Work

When Java reaches a method call:
1. It pauses the current method
2. Jumps to the called method and runs it
3. Returns to where it left off when the method finishes

```java
public static void main(String[] args) {
    System.out.println("Before");
    greet("Alice");               // jumps to greet, runs it, comes back
    System.out.println("After");
}
```

Output:
```
Before
Hello, Alice!
After
```

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `printStars(4, 5)` when method only takes 1 param | Wrong number of arguments | Match the parameter count |
| `printStars("four")` | Wrong argument type | Pass an `int`, not a `String` |
| Calling a method before defining it at the class level | Doesn't apply in Java — order within the class doesn't matter | Methods can be defined in any order |
| Forgetting parentheses: `greet` instead of `greet("Alice")` | Won't compile | Always include `()` when calling |

---

## Check Your Understanding

!!! information

    **Unit 2 · Chapter 1**

    ### Part A: Concepts

    **1.** Name three benefits of using methods instead of copying code.

    **2.** What does `void` mean in a method header?

    **3.** What is the difference between a **parameter** and an **argument**?

    ---

    ### Part B: Predict the Output

    **4.**
    ```java
    public static void shout(String word) {
        System.out.println(word.toUpperCase() + "!!!");
    }

    public static void main(String[] args) {
        shout("hello");
        shout("java");
    }
    ```

    **5.**
    ```java
    public static void printLine(int n, String ch) {
        for (int i = 0; i < n; i++) {
            System.out.print(ch);
        }
        System.out.println();
    }

    public static void main(String[] args) {
        printLine(4, "*");
        printLine(3, "-");
        printLine(4, "*");
    }
    ```

    ---

    ### Part C: Write the Code

    **6.** Write a void method `printBox` that takes an `int size` and prints a filled square of `*` characters. `printBox(3)` should print:
    ```
    * * *
    * * *
    * * *
    ```

    **7.** Write a `main` method that calls `printBox` with sizes 2, 4, and 2, with a blank line between each box.

    ---

    ## Answer Key

    ### Part A

    **1.** DRY (write once, reuse), decomposition (break big problems into named pieces), readability (main reads like an outline).

    **2.** The method performs an action but does not return a value to the caller.

    **3.** A **parameter** is the variable declared in the method header (`int n`). An **argument** is the actual value passed when the method is called (`printStars(4)` — `4` is the argument).

    ### Part B

    **4.**
    ```
    HELLO!!!
    JAVA!!!
    ```

    **5.**
    ```
    ****
    ---
    ****
    ```

    ### Part C

    **6.**
    ```java
    public static void printBox(int size) {
        for (int row = 0; row < size; row++) {
            for (int col = 0; col < size; col++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
    ```

    **7.**
    ```java
    public static void main(String[] args) {
        printBox(2);
        System.out.println();
        printBox(4);
        System.out.println();
        printBox(2);
    }
    ```

---

## Homework 5: Introduction to Methods

!!! attention

    **Unit 2 · Chapter 1**

    *Assigned Class 6 · Due Class 7*

    ### Part 1: Why Methods?

    Read the two versions of the same program, then answer the questions.

    **Version A — no methods:**
    ```java
    public static void main(String[] args) {
        System.out.println("==========");
        System.out.println("Welcome!");
        System.out.println("==========");
        System.out.println("Round 1 starting...");
        System.out.println("==========");
        System.out.println("Welcome!");
        System.out.println("==========");
        System.out.println("Round 2 starting...");
        System.out.println("==========");
        System.out.println("Welcome!");
        System.out.println("==========");
    }
    ```

    **Version B — with a method:**
    ```java
    public static void main(String[] args) {
        printBanner();
        System.out.println("Round 1 starting...");
        printBanner();
        System.out.println("Round 2 starting...");
        printBanner();
    }

    public static void printBanner() {
        System.out.println("==========");
        System.out.println("Welcome!");
        System.out.println("==========");
    }
    ```

    1. Both versions produce identical output. If you needed to change `"Welcome!"` to `"Welcome back!"`, how many lines would you edit in Version A? How many in Version B?
    2. Version B uses a method to avoid repeating code. What is the programming term for this benefit?
    3. A large program might have a `main` like this:
    ```java
    public static void main(String[] args) {
        getUserInput();
        validateInput();
        processData();
        displayResults();
    }
    ```
    You don't need to know what each method does to understand the program's structure. What is the term for breaking a problem into smaller named pieces like this?

    ### Part 2: Reading Void Methods

    A void method performs an action but does not return a value.

    4. What does the following method do? Describe it in one sentence without using the words "print" or "output."
    ```java
    public static void printStars(int n) {
        for (int i = 0; i < n; i++) {
            System.out.print("* ");
        }
        System.out.println();
    }
    ```
    5. What would `printStars(4)` display?
    6. Write a method call (not the method itself) that would display a row of 7 stars.
