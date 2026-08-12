# CS2 — Unit 1: Putting It All Together

This page ties together everything from Unit 1 before Quiz 1: Java review (variables, printing, operators, conditionals), loop review (`while` and `for`), and arrays (declaring, indexing, `.length`, and looping over elements). No new material here — just a recap and a chance to practice before the quiz.

---

## Quick Recap

**Java basics (1.1):** variables and data types, `System.out.println`/`print`, arithmetic and comparison operators, `if`/`else if`/`else` chains.

**Loops (1.2):** `while` loops check a condition before every pass; `for` loops pack initialization, condition, and update into one line. Both repeat a block of code — the choice is about which fits the situation better.

**Arrays (1.3):** a fixed-size, same-type collection, indexed from `0` to `length - 1`. You can loop over one with a `for` loop:

```java
for (int i = 0; i < arr.length; i++) {
    // do something with arr[i]
}
```

That's the one array skill Quiz 1 will check — reading/printing every element with a loop. Real array *algorithms* (totals, counts, finding values) are coming up during Project 1.

---

## Homework 4: Review

!!! attention

    **Unit 1 · Putting It All Together**

    *Assigned Class 4 · Due Class 5*

    A mixed review set — no new material, just practice for Quiz 1.

    1. Predict the output.
    ```java
    int score = 76;
    if (score >= 90) {
        System.out.println("A");
    } else if (score >= 80) {
        System.out.println("B");
    } else if (score >= 70) {
        System.out.println("C");
    } else {
        System.out.println("F");
    }
    ```

    2. Predict the output.
    ```java
    int total = 0;
    for (int i = 1; i <= 4; i++) {
        total += i * 2;
    }
    System.out.println(total);
    ```

    3. Predict the output.
    ```java
    int n = 20;
    int count = 0;
    while (n > 1) {
        n /= 2;
        count++;
    }
    System.out.println(n + " " + count);
    ```

    4. Write a single statement to declare and initialize a `double` array called `weights` containing the values `2.5, 4.0, 1.75, 3.25`.

    5. Given `int[] ids = {40, 41, 42, 43, 44};`, what is `ids[ids.length - 1]`?

    6. Write a `for` loop that prints every element of `String[] words = {"quiz", "prep", "time"};`, one per line.

    7. Find the bug.
    ```java
    int[] nums = {5, 10, 15};
    for (int i = 0; i <= nums.length; i++) {
        System.out.println(nums[i]);
    }
    ```

    8. Find the bug.
    ```java
    int x = 0;
    while (x < 5) {
        System.out.println(x);
    }
    ```
