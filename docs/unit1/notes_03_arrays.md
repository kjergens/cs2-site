# CS2 — Unit Notes: Introduction to Arrays

An **array** is a fixed-size collection of values of the same type, stored in numbered slots. Arrays are your first data structure in CS2 and the foundation for almost every algorithm you'll write this semester.

---

## 1. What Is an Array?

Imagine you need to store five quiz scores. Without arrays:

```java
int score0 = 88;
int score1 = 72;
int score2 = 95;
int score3 = 61;
int score4 = 83;
```

With an array:

```java
int[] scores = {88, 72, 95, 61, 83};
```

One variable, five values. And when you have 30 scores instead of 5, the array scales — the five separate variables don't.

**Key properties of arrays:**
- All elements must be the **same type**
- Size is **fixed at creation** — you cannot add or remove slots
- Elements are numbered starting at **index 0**

---

## 2. Declaring and Initializing

### Method 1 — Initializer list (when you know the values)

```java
int[] temps = {72, 68, 85, 90, 77};
String[] days = {"Mon", "Tue", "Wed", "Thu", "Fri"};
double[] prices = {4.99, 12.50, 7.25};
```

The size is inferred from the number of values you provide.

### Method 2 — New with size (when you know the size but not the values yet)

```java
int[] scores = new int[6];    // 6 slots, all initialized to 0
```

Default values by type: `int` → `0`, `double` → `0.0`, `boolean` → `false`, `String` → `null`

You then assign values individually:

```java
scores[0] = 90;
scores[1] = 85;
// etc.
```

**You cannot mix these:** `{...}` syntax only works at declaration, not after:

```java
int[] scores = new int[3];
scores = {90, 85, 78};    // ERROR — won't compile
```

---

## 3. Accessing and Modifying Elements

Use square brackets with an index to read or write a specific slot:

```java
String[] colors = {"red", "green", "blue"};

System.out.println(colors[0]);    // red
System.out.println(colors[1]);    // green

colors[1] = "yellow";             // replace "green" with "yellow"
System.out.println(colors[1]);    // yellow
```

Indices run from `0` to `length - 1`. Accessing outside that range throws an **ArrayIndexOutOfBoundsException** at runtime:

```java
System.out.println(colors[3]);    // ERROR — valid indices are 0, 1, 2
```

---

## 4. The `.length` Property

`.length` gives the number of slots in the array. It is a **property**, not a method — no parentheses:

```java
int[] nums = {4, 8, 15, 16, 23};
System.out.println(nums.length);     // 5
System.out.println(nums[nums.length - 1]);   // 23 — the last element
```

Common trap: `nums.length` is 5, but `nums[5]` doesn't exist. The last valid index is always `length - 1`.

---

## 5. Traversing an Array

The standard pattern for visiting every element:

```java
int[] scores = {88, 72, 95, 61, 83};

for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}
```

Why `i < scores.length` and not `i <= scores.length`? Because `scores.length` is 5, and `scores[5]` doesn't exist. Use `<`, not `<=`.

### Traversing in reverse

```java
for (int i = scores.length - 1; i >= 0; i--) {
    System.out.print(scores[i] + " ");
}
// prints: 83 61 95 72 88
```

---

## 6. Common Array Algorithms

These four patterns appear constantly — in HW, on quizzes, and in real programs. Memorize their shapes.

### Sum

```java
int[] prices = {15, 42, 8, 27, 63};
int sum = 0;
for (int i = 0; i < prices.length; i++) {
    sum += prices[i];
}
System.out.println(sum);   // 155
```

### Maximum

Initialize with the first element — never with `0`:

```java
int[] scores = {88, 72, 95, 61, 83};
int max = scores[0];
for (int i = 1; i < scores.length; i++) {
    if (scores[i] > max) {
        max = scores[i];
    }
}
System.out.println(max);   // 95
```

Why not `max = 0`? If every value in the array were negative, `0` would be returned as the max even though it's not in the array. Starting with `scores[0]` guarantees the answer is always a real element.

### Minimum

Same pattern as max, flip the comparison:

```java
int min = scores[0];
for (int i = 1; i < scores.length; i++) {
    if (scores[i] < min) {
        min = scores[i];
    }
}
System.out.println(min);   // 61
```

### Average

Sum first, then divide — and watch the type:

```java
double[] temps = {98.6, 101.2, 99.4, 103.0, 97.8};
double sum = 0;
for (int i = 0; i < temps.length; i++) {
    sum += temps[i];
}
System.out.println(sum / temps.length);   // 100.0
```

### Count with condition

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

---

## 7. Common Errors

| Error | Example | Problem | Fix |
|---|---|---|---|
| Off-by-one (upper bound) | `i <= arr.length` | `arr[arr.length]` doesn't exist | Use `i < arr.length` |
| Off-by-one (lower bound) | `i = 1` | Skips the first element | Start at `i = 0` |
| Bad max initialization | `int max = 0` | Fails for all-negative arrays | Use `max = arr[0]` |
| Post-declaration initializer | `scores = {90, 85};` | `{...}` only works at declaration | Combine into one statement or assign individually |
| Out of bounds access | `arr[arr.length]` | Last valid index is `length - 1` | Use `arr[arr.length - 1]` |

---

## Check Your Understanding

### Part A: Concepts

**1.** What is the index of the first element of any array? What is the index of the last element of an array with 8 elements?

**2.** What is the difference between `arr.length` and `arr.length()`?

**3.** Why should you initialize `max` with `arr[0]` instead of `0`?

**4.** You need to store 10 student names but you don't know them yet. Write a declaration that creates the array with room for 10 Strings.

**5.** True or false — explain in one sentence:
- a) You can change a value in an array after it is created.
- b) You can add a new slot to an array after it is created.

---

### Part B: Predict the Output

**6.**
```java
int[] nums = {4, 8, 15, 16, 23};
System.out.println(nums[0]);
System.out.println(nums[nums.length - 1]);
System.out.println(nums.length);
```

**7.**
```java
int[] vals = {3, 7, 2, 9};
int total = 0;
for (int i = 0; i < vals.length; i++) {
    total += vals[i];
}
System.out.println(total);
```

**8.**
```java
String[] words = {"cat", "dog", "bird"};
for (int i = words.length - 1; i >= 0; i--) {
    System.out.print(words[i] + " ");
}
```

---

### Part C: Write the Code

**9.** Given `int[] data = {12, 5, 8, 19, 3, 14}`, write a loop that prints only the values greater than 10.

**10.** Given `int[] scores = {74, 88, 91, 63, 85}`, write a loop that finds and prints the minimum value.

---

### Part D: Find the Bug

**11.**
```java
int[] nums = {10, 20, 30, 40, 50};
for (int i = 0; i <= nums.length; i++) {
    System.out.println(nums[i]);
}
```

**12.**
```java
int[] temps = {-5, -12, -3, -8};
int max = 0;
for (int i = 0; i < temps.length; i++) {
    if (temps[i] > max) {
        max = temps[i];
    }
}
System.out.println("Max: " + max);
```

---
---

## Answer Key

### Part A

**1.** First element is always index `0`. Last element of an 8-element array is index `7` (`length - 1`).

**2.** `arr.length` is a property — no parentheses, used for arrays. `arr.length()` is a method call — used for Strings. Using `()` on an array is a compile error.

**3.** If all values in the array are negative, `max = 0` will never be updated by the loop (no element is greater than 0), and the result will be `0` — which isn't in the array. Starting with `arr[0]` guarantees the answer is always a real element.

**4.** `String[] names = new String[10];`

**5.**
- a) **True** — you can assign a new value to any index at any time.
- b) **False** — array size is fixed at creation. Use `ArrayList` for a resizable collection.

### Part B

**6.**
```
4
23
5
```

**7.**
```
21
```
3 + 7 + 2 + 9 = 21.

**8.**
```
bird dog cat 
```
Loop runs from index 2 down to 0, printing in reverse.

### Part C

**9.**
```java
for (int i = 0; i < data.length; i++) {
    if (data[i] > 10) {
        System.out.println(data[i]);
    }
}
// prints: 12, 19, 14
```

**10.**
```java
int min = scores[0];
for (int i = 1; i < scores.length; i++) {
    if (scores[i] < min) {
        min = scores[i];
    }
}
System.out.println(min);   // 63
```

### Part D

**11.** The condition `i <= nums.length` allows `i` to reach `5`, but `nums[5]` doesn't exist (valid indices: 0–4). Throws `ArrayIndexOutOfBoundsException`. Fix: `i < nums.length`.

**12.** `max = 0` is the wrong initialization for an all-negative array. Every element is less than 0, so the loop never updates `max`, and the result is `0` — not in the array. Fix: `int max = temps[0]` and start the loop at `i = 1`.
