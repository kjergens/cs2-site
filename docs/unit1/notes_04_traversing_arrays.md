# CS2 — Unit 1 Chapter 4: Traversing Arrays
Traversal means visiting every element in an array, one at a time, using a loop. Traversal is how you compute results from arrays: totals, averages, finding the largest value, counting elements that match a condition.

---

## 1. The Traversal Template

```java
for (int i = 0; i < arr.length; i++) {
    // do something with arr[i]
}
```

Every array algorithm in this unit is a variation on this template. What changes is what happens inside the loop body.

---

## 2. The Five Core Algorithms

These five patterns appear constantly — in homework, on quizzes, and in real programs. Know them cold.

---

### 2.1 Sum

Add every element to a running total.

```java
int[] prices = {15, 42, 8, 27, 63, 11};
int sum = 0;
for (int i = 0; i < prices.length; i++) {
    sum += prices[i];
}
System.out.println(sum);   // 166
```

**Pattern:** declare accumulator before the loop, add inside.

---

### 2.2 Maximum

Find the largest value. Initialize with the first element — never `0`.

```java
int[] scores = {88, 72, 95, 61, 83, 97, 74};
int max = scores[0];
for (int i = 1; i < scores.length; i++) {
    if (scores[i] > max) {
        max = scores[i];
    }
}
System.out.println(max);   // 97
```

**Why `scores[0]` and not `0`?** If every value were negative, `max = 0` would never be updated and the result would be `0` — not in the array. Starting with a real element guarantees a correct answer regardless of the values.

**Why start the loop at `i = 1`?** Because `scores[0]` is already accounted for as the initial value of `max`.

---

### 2.3 Minimum

Same structure as max — flip the comparison.

```java
int min = scores[0];
for (int i = 1; i < scores.length; i++) {
    if (scores[i] < min) {
        min = scores[i];
    }
}
System.out.println(min);   // 61
```

---

### 2.4 Average

Sum first, then divide. Watch the type: dividing two `int`s gives an `int` result.

```java
int[] scores = {88, 72, 95, 61, 83};
int sum = 0;
for (int i = 0; i < scores.length; i++) {
    sum += scores[i];
}
double avg = (double) sum / scores.length;
System.out.println(avg);   // 79.8
```

The cast `(double) sum` forces decimal division before the result is stored. Without it: `399 / 5 = 79` (integer division drops the decimal).

---

### 2.5 Count with Condition

Count how many elements satisfy a test.

```java
int[] grades = {55, 78, 92, 61, 88, 45, 73, 90};
int count = 0;
for (int i = 0; i < grades.length; i++) {
    if (grades[i] >= 80) {
        count++;
    }
}
System.out.println(count);   // 3
```

**Pattern:** counter starts at 0, increments inside an `if`.

---

## 3. Combining Algorithms

You can compute multiple results in a single pass:

```java
int[] scores = {88, 72, 95, 61, 83};
int sum = 0;
int max = scores[0];
for (int i = 0; i < scores.length; i++) {
    sum += scores[i];
    if (scores[i] > max) {
        max = scores[i];
    }
}
System.out.println("Sum: " + sum);           // 399
System.out.println("Max: " + max);           // 95
System.out.println("Avg: " + (double) sum / scores.length);  // 79.8
```

---

## 4. Variations

### Sum of elements that meet a condition

```java
int[] nums = {4, 9, 2, 7, 1, 6};
int evenSum = 0;
for (int i = 0; i < nums.length; i++) {
    if (nums[i] % 2 == 0) {
        evenSum += nums[i];
    }
}
System.out.println(evenSum);   // 12  (4 + 2 + 6)
```

### Reverse traversal

```java
int[] vals = {10, 20, 30, 40, 50};
for (int i = vals.length - 1; i >= 0; i--) {
    System.out.print(vals[i] + " ");
}
// 50 40 30 20 10
```

### Finding an element's index

```java
int[] ids = {102, 57, 88, 34, 71};
int target = 88;
int foundAt = -1;
for (int i = 0; i < ids.length; i++) {
    if (ids[i] == target) {
        foundAt = i;
    }
}
System.out.println(foundAt);   // 2
```

If `foundAt` is still `-1` after the loop, the target wasn't in the array.

---

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `i <= arr.length` | Out of bounds on last iteration | `i < arr.length` |
| `int max = 0` | Wrong result for all-negative arrays | `int max = arr[0]` |
| `int sum / scores.length` | Integer division drops decimal | Cast: `(double) sum / scores.length` |
| Accumulator inside the loop | Resets to 0 every iteration | Declare `sum = 0` before the loop |

---

## Check Your Understanding

!!! information

    **Unit 1 · Chapter 4**

    ### Part A: Concepts

    **1.** Why must the accumulator (`sum`, `max`, etc.) be declared before the loop, not inside it?

    **2.** What is wrong with initializing `min = 0` for a minimum-finding algorithm?

    **3.** You have `int[] data` and want to compute the average as a decimal. Write the one line that performs the division correctly.

    ---

    ### Part B: Predict the Output

    **4.**
    ```java
    int[] nums = {4, 9, 2, 7, 1, 6};
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] % 2 == 0) {
            sum += nums[i];
        }
    }
    System.out.println(sum);
    ```

    **5.**
    ```java
    int[] vals = {10, 30, 20, 50, 40};
    int max = vals[0];
    for (int i = 1; i < vals.length; i++) {
        if (vals[i] > max) {
            max = vals[i];
        }
    }
    System.out.println(max);
    ```

    **6.**
    ```java
    int[] scores = {70, 80, 90};
    int sum = 0;
    for (int i = 0; i < scores.length; i++) {
        sum += scores[i];
    }
    System.out.println(sum / scores.length);
    System.out.println((double) sum / scores.length);
    ```

    ---

    ### Part C: Write the Code

    **7.** Given `int[] temps = {64, 71, 58, 82, 76, 55, 69}`, write a loop that counts and prints how many temperatures are below 70.

    **8.** Given `int[] vals = {12, 5, 8, 19, 3, 14}`, write a loop that finds and prints both the minimum AND the maximum in a single pass.

    ---
    ---

    ## Answer Key

    ### Part A

    **1.** If declared inside the loop, the variable is re-created and reset to its starting value every iteration. The accumulated result is lost each time.

    **2.** If every element is positive, it happens to work — but only by accident. If all values were negative (e.g., temperatures in Celsius), `0` would be returned as the minimum even though it's not in the array. Always start with `arr[0]`.

    **3.** `System.out.println((double) sum / data.length);`

    ### Part B

    **4.** `12` — even numbers in the array: 4, 2, 6 → 4 + 2 + 6 = 12.

    **5.** `50` — max starts at 10, gets updated to 30, then 50. 40 doesn't beat 50.

    **6.**
    ```
    80
    80.0
    ```
    First line: `240 / 3 = 80` (integer division, happens to be exact here). Second line: `(double) 240 / 3 = 80.0`.

    ### Part C

    **7.**
    ```java
    int count = 0;
    for (int i = 0; i < temps.length; i++) {
        if (temps[i] < 70) {
            count++;
        }
    }
    System.out.println(count);   // 4 (64, 58, 55, 69)
    ```

    **8.**
    ```java
    int min = vals[0];
    int max = vals[0];
    for (int i = 1; i < vals.length; i++) {
        if (vals[i] < min) {
            min = vals[i];
        }
        if (vals[i] > max) {
            max = vals[i];
        }
    }
    System.out.println("Min: " + min);   // 3
    System.out.println("Max: " + max);   // 19
    ```

---

## Homework 4: Arrays and Loops

!!! attention

    **Unit 1 · Chapter 4**

    *Assigned Class 4 · Due Class 5*

    ### Part 1: Traversal — Predict the Output

    1. Predict the output.
    ```java
    int[] vals = {10, 20, 30, 40, 50};
    int total = 0;
    for (int i = 0; i < vals.length; i++) {
        total += vals[i];
    }
    System.out.println(total);
    ```

    2. Predict the output.
    ```java
    int[] data = {5, 12, 3, 8, 15, 6};
    int count = 0;
    for (int i = 0; i < data.length; i++) {
        if (data[i] > 7) {
            count++;
        }
    }
    System.out.println(count);
    ```

    ### Part 2: Write the Algorithms

    For each problem, write a complete for loop (or loops) that works on the given array. Do not change the array declaration.

    3. **Sum**
    ```java
    int[] prices = {15, 42, 8, 27, 63, 11};
    // Write a loop that calculates and prints the sum of all elements.
    ```

    4. **Maximum**
    ```java
    int[] scores = {88, 72, 95, 61, 83, 97, 74};
    // Write a loop that finds and prints the largest value.
    // Hint: start by assuming the first element is the max.
    ```

    5. **Minimum**
    ```java
    int[] scores = {88, 72, 95, 61, 83, 97, 74};
    // Write a loop that finds and prints the smallest value.
    ```

    6. **Count with condition**
    ```java
    int[] grades = {55, 78, 92, 61, 88, 45, 73, 90};
    // Write a loop that counts and prints how many grades are 80 or above.
    ```

    7. **Average**
    ```java
    double[] temps = {98.6, 101.2, 99.4, 103.0, 97.8};
    // Write a loop that calculates and prints the average temperature.
    // Print the result as a double.
    ```

    ### Part 3: Find the Bug

    Each snippet has exactly one error. Identify the line and describe the problem.

    8. Find the bug.
    ```java
    int[] nums = {4, 8, 15, 16, 23};
    for (int i = 1; i <= nums.length; i++) {
        System.out.println(nums[i]);
    }
    ```

    9. This compiles and runs — but gives the wrong answer for some inputs. What is the flaw?
    ```java
    int[] data = {5, 3, 9, 1, 7};
    int max = 0;
    for (int i = 0; i < data.length; i++) {
        if (data[i] > max) {
            max = data[i];
        }
    }
    System.out.println("Max: " + max);
    ```

    10. Find the bug.
    ```java
    int[] scores = new int[5];
    scores = {90, 85, 78, 92, 88};
    System.out.println(scores[0]);
    ```
