# CS2 — Unit 2: Putting It All Together

This page ties together everything from Unit 2 before Quiz 2: methods (input/output, parameters, return values), `void` methods, choosing between the two, and arrays as method parameters. No new material here — just a recap and a chance to practice before the quiz.

---

## Quick Recap

**Methods: input, output (2.1):** a method takes input through its parameters and hands back output through `return` — same idea as a math function `f(x)`. Java passes primitives by value (a method gets its own copy), and parameters/local variables only exist inside their own method. A method with a return type (not `void`) must return a value along every possible path; the caller decides what to do with the result — print it, store it, use it in another expression.

**Void methods (2.2):** when a method's whole job is an action — usually printing — it has nothing to hand back, so its return type is `void`. Parameters work exactly the same as in any other method; the only difference is there's no value to store or reuse afterward.

**Choosing void vs. a return value (2.3):** prefer a return value whenever the result needs to be reused, stored, or combined with something else — a `void` result can't be. Use `void` when the method's entire job is the action itself.

**Arrays as parameters (2.4):** unlike primitives, arrays are passed by reference — a method that modifies array elements changes the original array. Methods can also read an array and compute/return a result (sum, max, average, etc.) without modifying it.

---

## Homework 9: Review

!!! attention

    **Unit 2 · Putting It All Together**

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
    public static void main(String[] args) {
        int[] nums = {4, 7, 2};
        System.out.println(sumArray(nums));
    }
    
    public static int sumArray(int[] arr) {
        int total = 0;
        for (int i = 0; i < arr.length; i++) {
            total += arr[i];
        }
        return total;
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

    8. For an array, for example `String[] words = {"cat", "dog", "fish"}`, what is `words.length`?

    9. For this code:
    ```java
    public static void main(String[] args) {
        double price = 19.99;
        String[] items = {"Book", "Pen", "Bag"};
        String[] cart = items;
    }
    ```

      * a. The primitive variable `price` is stored on the: ______________________
      * b. The reference variable `cart` is stored on the stack, but the actual sequence of String objects it points to is stored on the: ______________________
      * c. If you execute `cart[2] = "Desk";`, what will be the value of `items[2]` immediately after? ______________________

    10. What is printed by the following program? Trace your memory carefully!
    ```java
    public static void main(String[] args) {
        int[] values = {5, 10, 15};
        alterArray(values);
        System.out.println(values[0] + " " + values[1] + " " + values[2]);
    }

    public static void alterArray(int[] data) {
        data = new int[]{100, 200, 300};
        data[0] = 999;
    }
    ```
    
    
