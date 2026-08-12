# CS2 — Unit 2: Putting It All Together

This page ties together everything from Unit 2 before Quiz 2: methods (why they exist, `void` vs. non-`void`), parameters and local scope, return values, and arrays as method parameters. No new material here — just a recap and a chance to practice before the quiz.

---

## Quick Recap

**Why methods (2.1):** decomposition and reuse — instead of repeating code, wrap it in a method and call it by name. `void` methods perform an action and return nothing; non-`void` methods compute and hand back a value.

**Parameters and scope (2.2):** Java passes primitives by value — a method gets its own copy, so changes inside the method don't affect the caller's variable. Variables declared inside a method (or any `{ }` block) only exist within that block.

**Return values (2.3):** a method with a return type (not `void`) must return a value along every possible path. The caller decides what to do with the result — print it, store it, use it in another expression.

**Arrays as parameters (2.4):** unlike primitives, arrays are passed by reference — a method that modifies array elements changes the original array. Methods can also read an array and compute/return a result (sum, max, average, etc.) without modifying it.

---

## Homework 9: Review

!!! attention

    **Unit 2 · Putting It All Together**

    *Assigned Class 10 · Due Class 11*

    A mixed review set — no new material, just practice for Quiz 2.

    1. Predict the output.
    ```java
    public static void printBanner() {
        System.out.println("====");
        System.out.println("Hi!");
        System.out.println("====");
    }

    public static void main(String[] args) {
        printBanner();
        System.out.println("Round 1");
    }
    ```

    2. Predict the output.
    ```java
    public static void addFive(int n) {
        n = n + 5;
        System.out.println("Inside method: n = " + n);
    }

    public static void main(String[] args) {
        int x = 10;
        addFive(x);
        System.out.println("After: " + x);
    }
    ```

    3. Predict the output.
    ```java
    public static int cube(int n) {
        return n * n * n;
    }

    public static void main(String[] args) {
        System.out.println(cube(3));
        System.out.println(cube(2) + cube(1));
    }
    ```

    4. Write a method called `isPositive` that takes an `int` parameter and returns `true` if it's greater than `0`, `false` otherwise.

    5. What is printed by the following code?
    ```java
    public static int sumArray(int[] arr) {
        int total = 0;
        for (int n : arr) {
            total += n;
        }
        return total;
    }

    public static void main(String[] args) {
        int[] nums = {4, 7, 2};
        System.out.println(sumArray(nums));
    }
    ```

    6. Find the bug.
    ```java
    public static int square(int n) {
        int result = n * n;
    }
    ```

    7. Find the bug.
    ```java
    public static void main(String[] args) {
        printMessage();
        System.out.println(msg);
    }

    public static void printMessage() {
        String msg = "Hello!";
        System.out.println(msg);
    }
    ```

    8. In one sentence, what's the difference between a `void` method and a method that returns a value?
