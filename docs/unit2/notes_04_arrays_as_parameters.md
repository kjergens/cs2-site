# CS2 — Unit Notes: Passing Arrays to Methods

---

## 1. Arrays Are Different from Primitives

When you pass a **primitive** (int, double, boolean) to a method, the method gets a copy. Changes to the copy don't affect the original.

When you pass an **array**, the method gets a **reference** — a pointer to the same array in memory. Changes the method makes to array elements *do* affect the original.

```java
public static void doubleFirst(int[] arr) {
    arr[0] = arr[0] * 2;
}

public static void main(String[] args) {
    int[] nums = {5, 10, 15};
    doubleFirst(nums);
    System.out.println(nums[0]);   // 10 — the original was changed!
}
```

Compare with a primitive:
```java
public static void tryToChange(int n) {
    n = n * 2;
}

public static void main(String[] args) {
    int x = 5;
    tryToChange(x);
    System.out.println(x);   // still 5 — copy unchanged
}
```

**The rule:**
- Primitives → passed by **value** (copy) → original unchanged
- Arrays → passed by **reference** → method can modify the original

---

## 2. Void Methods That Modify Arrays

A void method can take an array and modify its elements in place:

```java
public static void addOne(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        arr[i] += 1;
    }
}

public static void main(String[] args) {
    int[] data = {10, 20, 30};
    addOne(data);
    for (int i = 0; i < data.length; i++) {
        System.out.print(data[i] + " ");   // 11 21 31
    }
}
```

The method uses no `return` — it modifies the array through the reference.

---

## 3. Methods That Take Arrays and Return Values

A method can take an array, compute something, and return the result:

```java
public static int sum(int[] arr) {
    int total = 0;
    for (int i = 0; i < arr.length; i++) {
        total += arr[i];
    }
    return total;
}

public static double average(int[] arr) {
    return (double) sum(arr) / arr.length;
}
```

**Calling them:**
```java
int[] scores = {88, 72, 95, 61, 83};
System.out.println(sum(scores));       // 399
System.out.println(average(scores));   // 79.8
```

Notice: `average` calls `sum` — one method can call another. This is decomposition in action.

---

## 4. Void vs Non-Void with Arrays — Summary

| Method | Does what | Returns |
|---|---|---|
| `void addOne(int[] arr)` | Modifies array elements in place | Nothing |
| `int sum(int[] arr)` | Computes a value from the array | The value |
| `int max(int[] arr)` | Finds the largest element | The value |
| `void normalize(int[] arr, int d)` | Divides every element by d in place | Nothing |

Use **void** when the method's job is to modify the array.  
Use a **return type** when the method's job is to compute something from the array.

---

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `i <= arr.length` in loop | Off by one — crashes last iteration | `i < arr.length` |
| `return total / arr.length` when average should be double | Integer division drops decimal | `return (double) total / arr.length` |
| Expecting array to be unchanged after passing to a method | Arrays pass by reference — method can modify them | Check whether the method modifies elements |
| Expecting `int x = 5` to change after method call | Primitives pass by copy | Primitives are safe from modification |

---

## Check Your Understanding

### Part A: Concepts

**1.** What is the difference between passing a primitive and passing an array to a method?

**2.** A method has signature `public static void fill(int[] arr, int val)`. After calling `fill(data, 0)`, what do you expect `data` to contain?

---

### Part B: Predict the Output

**3.**
```java
public static void doubleAll(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        arr[i] *= 2;
    }
}
public static void main(String[] args) {
    int[] nums = {3, 5, 7};
    doubleAll(nums);
    for (int i = 0; i < nums.length; i++) {
        System.out.print(nums[i] + " ");
    }
}
```

**4.**
```java
public static int sum(int[] arr) {
    int total = 0;
    for (int i = 0; i < arr.length; i++) {
        total += arr[i];
    }
    return total;
}
public static void main(String[] args) {
    int[] vals = {10, 20, 30};
    System.out.println(sum(vals));
    System.out.println(vals[0]);
}
```

---

### Part C: Write the Code

**5.** Write a method `max(int[] arr)` that returns the largest element.

**6.** Write a method `countAbove(int[] arr, int threshold)` that returns how many elements are strictly greater than `threshold`.

---
---

## Answer Key

### Part A

**1.** Primitives are passed by value — the method gets a copy and cannot change the original. Arrays are passed by reference — the method works on the same array, so changes to elements persist after the method returns.

**2.** Every element of `data` would be set to 0.

### Part B

**3.** `6 10 14 ` — `doubleAll` modifies the original array through the reference.

**4.**
```
60
10
```
`sum` reads the array but does not modify it. `vals[0]` is still 10.

### Part C

**5.**
```java
public static int max(int[] arr) {
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) { max = arr[i]; }
    }
    return max;
}
```

**6.**
```java
public static int countAbove(int[] arr, int threshold) {
    int count = 0;
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] > threshold) { count++; }
    }
    return count;
}
```
