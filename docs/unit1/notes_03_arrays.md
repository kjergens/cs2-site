# CS2 — Unit 1 Chapter 3: Arrays
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

You already know `for` loops from Chapter 2, and you now know how to index into an array — combine them and you can visit every element with a loop:

```java
int[] nums = {4, 8, 15, 16, 23};
for (int i = 0; i < nums.length; i++) {
    System.out.println(nums[i]);
}
```

That's all traversal is at its core: a `for` loop whose index runs from `0` to `arr.length - 1`, using that index to read each slot. Project 1 will build on this with real algorithms (totals, counts, finding values) — for now, just get comfortable with the pattern above.

---

## 5. Common Errors

| Error | Example | Problem | Fix |
|---|---|---|---|
| Post-declaration initializer | `scores = {90, 85};` | `{...}` only works at declaration | Combine into one statement or assign individually |
| Out of bounds access | `arr[arr.length]` | Last valid index is `length - 1` | Use `arr[arr.length - 1]` |

---

## Check Your Understanding

!!! information

    **Unit 1 · Chapter 3**

    ### Part A: Concepts

    **1.** What is the index of the first element of any array? What is the index of the last element of an array with 8 elements?

    **2.** What is the difference between `arr.length` and `arr.length()`?

    **3.** You need to store 10 student names but you don't know them yet. Write a declaration that creates the array with room for 10 Strings.

    **4.** True or false — explain in one sentence:
    - a) You can change a value in an array after it is created.
    - b) You can add a new slot to an array after it is created.

    ---

    ### Part B: Predict the Output

    **5.**
    ```java
    int[] nums = {4, 8, 15, 16, 23};
    System.out.println(nums[0]);
    System.out.println(nums[nums.length - 1]);
    System.out.println(nums.length);
    ```

    ---
    ---

    ## Answer Key

    ### Part A

    **1.** First element is always index `0`. Last element of an 8-element array is index `7` (`length - 1`).

    **2.** `arr.length` is a property — no parentheses, used for arrays. `arr.length()` is a method call — used for Strings. Using `()` on an array is a compile error.

    **3.** `String[] names = new String[10];`

    **4.**
    - a) **True** — you can assign a new value to any index at any time.
    - b) **False** — array size is fixed at creation. Use `ArrayList` for a resizable collection.

    ### Part B

    **5.**
    ```
    4
    23
    5
    ```

---

## Homework 3: Arrays

!!! attention

    **Unit 1 · Chapter 3**

    *Assigned Class 3 · Due Class 4*

    1. Write a single statement to declare and initialize an `int` array called `temps` containing the values `72, 68, 85, 90, 77`.

    2. Write two statements: first declare an `int` array called `scores` that can hold 6 values; then assign `100` to the last slot.

    3. Given:
    ```java
    String[] days = {"Mon", "Tue", "Wed", "Thu", "Fri"};
    ```
    Answer each without running the code:
       - a) What does `days[0]` return?
       - b) What does `days[days.length - 1]` return?
       - c) What happens if you access `days[5]`?

    4. True or false — explain your answer in one sentence.
       - a) You can change the value at `arr[2]` after the array is created.
       - b) You can change the size of an array after it is created.

    5. Using the `temps` array from Question 1, write a `for` loop that prints each temperature on its own line.
