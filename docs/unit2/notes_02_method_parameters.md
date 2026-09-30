# CS2 — Unit 2 Chapter 2: Void Methods

You've been writing methods that hand back a value, so `main` can decide what to do with it — print it, store it, use it in more math. **Sometimes you don't need anything handed back.** If all a method needs to do is *perform an action* — like printing something formatted a certain way — there's nothing to return.

That's a **void method**.

---

## 1. Same Idea, No Output

Compare two versions of "square something":

```java
// Returns a value — the caller decides what to do with it
public static int square(int n) {
    return n * n;
}

// void — just does the printing itself, hands nothing back
public static void printSquare(int n) {
    System.out.println(n * n);
}
```

```java
System.out.println(square(5));   // caller prints it
printSquare(5);                  // method prints it directly — same output, different approach
```

The word `void` where the return type goes means "nothing comes back." There's no `return` statement needed (though a bare `return;` with no value is legal, to exit early) — the method just runs its code and control returns to the caller when it hits the closing `}`.

| Part | Example | Meaning |
|---|---|---|
| Access modifier | `public` | Visible to the whole program |
| `static` | `static` | Belongs to the class, not an object |
| Return type | `void` | Nothing comes back |
| Method name | `printSquare` | What you call it |
| Parameter list | `int n` | Input(s), same as any other method |
| Body | `{ ... }` | The code that runs |

---

## 2. Parameters Work Exactly the Same Way

Void methods take parameters exactly like methods that return a value — nothing changes about how inputs work, only about what comes out the other side.

```java
public static void printMultiples(int n, int count) {
    for (int i = 1; i <= count; i++) {
        System.out.println(n * i);
    }
}
```

**Calling it:**
```java
printMultiples(3, 4);  // prints 3, 6, 9, 12 — one per line
```

Everything you already know still applies: arguments map to parameters in order, primitives are passed by value, and parameters are local to the method.

---

## 3. When to Reach for `void`

Use a return value whenever a method computes something the caller needs to use, store, or pass along. Use `void` when the method's entire job is an action with nothing left to hand back — most commonly, **printing**.

```java
public static void printBanner() {
    System.out.println("==========");
    System.out.println("Welcome!");
    System.out.println("==========");
}

public static void printBox(int size) {
    for (int row = 0; row < size; row++) {
        for (int col = 0; col < size; col++) {
            System.out.print("* ");
        }
        System.out.println();
    }
}
```

Neither of these computes a value worth handing back — their whole purpose is what they print. That's the signal to make them `void`.

---

## 4. How Method Calls Work

When Java reaches a method call — void or not — it pauses the current method, jumps to the called method and runs it, then returns to where it left off.

```java
public static void main(String[] args) {
    System.out.println("Before");
    printBanner();
    System.out.println("After");
}
```

Output:
```
Before
==========
Welcome!
==========
After
```

---

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `int x = printSquare(4);` | Void methods produce no value to store | Either make the method return a value, or don't try to capture one |
| `System.out.println(printBanner());` | Same issue — trying to use a void call as if it were a value | Just call `printBanner();` on its own line |
| Forgetting parentheses: `printBanner` instead of `printBanner();` | Won't compile | Always include `()` when calling |
| `printMultiples(4)` when the method needs two params | Compile error | Supply all required arguments |

---

## Check Your Understanding

!!! information

    **Unit 2 · Chapter 2: Void Methods**

    1. What does `void` mean in a method header?
    - Rewrite this returning method as a void method that prints its result directly instead:
    ```java
    public static int cube(int n) {
        return n * n * n;
    }
    ```
    - Predict the output.
    ```java
    public static void main(String[] args) {
        shout("hello");
        shout("java");
    }
    public static void shout(String word) {
        System.out.println(word.toUpperCase() + "!!!");
    }
    ```
    - Why can't you write `int x = printBanner();`?
    - Write a void method `printLine` that takes an `int n` and a `String ch`, and prints `ch` repeated `n` times followed by a newline. `printLine(4, "p")` should print `pppp`.

    ---

    **Answer Key**

    1. The method performs an action but does not return a value to the caller.

    -
    ```java
    public static void printCube(int n) {
        System.out.println(n * n * n);
    }
    ```
    - Output:
    ```
    HELLO!!!
    JAVA!!!
    ```
    - `printBanner` is `void` — it never produces a value, so there's nothing for `x` to store.

    -
    ```java
    public static void printLine(int n, String ch) {
        for (int i = 0; i < n; i++) {
            System.out.print(ch);
        }
        System.out.println();
    }
    ```

---

## Homework 6: Void Methods

!!! attention

    **Unit 2 · Chapter 2 Homework**

    ### Part 1: Return Value or Void?

    For each task, decide whether the method should return a value or be `void`, and say why.

    1. A method that computes the area of a circle.
    2. A method that prints a row of stars.
    3. A method that checks whether a number is prime.
    4. A method that prints a formatted receipt given a price and a quantity.

    ### Part 2: Reading and Predicting

    5. What does the following method do? Describe it in one sentence.
    ```java
    public static void printStars(int n) {
        for (int i = 0; i < n; i++) {
            System.out.print("* ");
        }
        System.out.println();
    }
    ```
    6. What would `printStars(4)` display?
    7. Predict the output.
    ```java
    public static void main(String[] args) {
        printLine(4, "p");
        printLine(3, "&");
    }
    public static void printLine(int n, String ch) {
        for (int i = 0; i < n; i++) {
            System.out.print(ch);
        }
        System.out.println();
    }
    ```

    ### Part 3: Local Scope Check

    8. Will this code compile? Explain why or why not.
    ```java
    public static void main(String[] args) {
        compute();
        System.out.println(result);
    }
    public static void compute() {
        int result = 42;
        System.out.println(result);
    }
    ```

    ### Part 4: Write the Method

    9. Write a void method `printBox` that takes an `int size` and prints a filled square of `*` characters. `printBox(3)` should print:
    ```
    * * *
    * * *
    * * *
    ```

    10. Write a `main` method that calls `printBox` with sizes 2, 4, and 2, with a blank line between each box.
