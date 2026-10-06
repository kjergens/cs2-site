# CS2 — Unit 2 Chapter 4: Passing Arrays to Methods

---

## 1. Arrays Are Different from Primitives

When you pass a **primitive** (int, double, boolean) to a method, the method gets a copy. Changes to the copy don't affect the original.

When you pass an **array**, the method gets a **reference** — a pointer to the same array in memory. Changes the method makes to array elements *do* affect the original.

```java
public static void main(String[] args) {
    int[] nums = {5, 10, 15};
    doubleFirst(nums);
    System.out.println(nums[0]);   // 10 — the original was changed!
}

public static void doubleFirst(int[] nums) {
    nums[0] = nums[0] * 2;
}
```

Compare with a primitive:
```java
public static void main(String[] args) {
    int x = 5;
    tryToChange(x);
    System.out.println(x);   // still 5 — copy unchanged
}

public static void tryToChange(int n) {
    n = n * 2;
}
```

**The rule:**
- Primitives → passed by **value** (copy) → original unchanged
- Arrays → passed by **reference** → method can modify the original

---

## 2. Void Methods That Modify Arrays

A void method can take an array and modify its elements in place:

```java
public static void main(String[] args) {
    int[] data = {10, 20, 30};
    addOne(data);
    for (int i = 0; i < data.length; i++) {
        System.out.print(data[i] + " ");   // 11 21 31
    }
}

public static void addOne(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        nums[i] += 1;
    }
}
```

The method uses no `return` — it modifies the array through the reference.

---

## 3. Methods That Take Arrays and Return Values

A method can take an array, compute something, and return the result:

```java
public static int sum(int[] nums) {
    int total = 0;
    for (int i = 0; i < nums.length; i++) {
        total += nums[i];
    }
    return total;
}

public static double average(int[] nums) {
    return (double) sum(nums) / nums.length;
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
| `void addOne(int[] nums)` | Modifies array elements in place | Nothing |
| `int sum(int[] nums)` | Computes a value from the array | The value |
| `int max(int[] nums)` | Finds the largest element | The value |
| `void normalize(int[] nums, int d)` | Divides every element by d in place | Nothing |

Use **void** when the method's job is to modify the array.  
Use a **return type** when the method's job is to compute something from the array.

---

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `i <= nums.length` in loop | Off by one — crashes last iteration | `i < nums.length` |
| `return total / nums.length` when average should be double | Integer division drops decimal | `return (double) total / nums.length` |
| Expecting array to be unchanged after passing to a method | Arrays pass by reference — method can modify them | Check whether the method modifies elements |
| Expecting `int x = 5` to change after method call | Primitives pass by copy | Primitives are safe from modification |

---

## Check Your Understanding

!!! information

    **Unit 2 · Chapter 4**

    **Part A: Concepts**

    1. What is the difference between passing a primitive and passing an array to a method?
    - A method has signature `public static void fill(int[] arr, int val)`. After calling `fill(data, 0)`, what do you expect `data` to contain?
    - Predict the Output
    ```java
    public static void main(String[] args) {
        int[] nums = {3, 5, 7};
        doubleAll(nums);
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i] + " ");
        }
    }
        
    public static void doubleAll(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            arr[i] *= 2;
        }
    }
    ```
    - Predict the Output
    ```java
    public static void main(String[] args) {
        int[] vals = {10, 20, 30};
        System.out.println(sum(vals));
        System.out.println(vals[0]);
    }

    public static int sum(int[] arr) {
        int total = 0;
        for (int i = 0; i < arr.length; i++) {
            total += arr[i];
        }
        return total;
    }
    ```
    - Write a method `max(int[] arr)` that returns the largest element.
    - Write a method `countAbove(int[] arr, int threshold)` that returns how many elements are strictly greater than `threshold`.
    - Write a method `normalize(int[] arr, int divisor)` that divides every element of the array by the divisor in place (modifying the original array — no return value). Then write a `main` call that uses it.

    ---

    **Answer Key**

    **Part A**
    1. Primitives are passed by value — the method gets a copy and cannot change the original. Arrays are passed by reference — the method works on the same array, so changes to elements persist after the method returns.
    - Every element of `data` would be set to 0.
    - `6 10 14 ` — `doubleAll` modifies the original array through the reference.
    - output:
    ```
    60
    10
    ```
    `sum` reads the array but does not modify it. `vals[0]` is still 10.
    - output:
    ```java
    public static int max(int[] arr) {
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) { max = arr[i]; }
        }
        return max;
    }
    ```
    - output:
    ```java
    public static int countAbove(int[] arr, int threshold) {
        int count = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > threshold) { count++; }
        }
        return count;
    }
    ```
    - output:
    ```java
    public static void normalize(int[] arr, int divisor) {
        for (int i = 0; i < arr.length; i++) {
            arr[i] = arr[i] / divisor;
        }
    }
    // Example call in main:
    // int[] data = {10, 20, 30};
    // normalize(data, 10);   // data is now {1, 2, 3}
    ```
---

## Homework 8: Arrays as Parameters

!!! attention

    **Unit 2 · Chapter 4**

    ### Part 1: What Happens Inside the Method?

    1. Trace this code. What does `main` print?
    ```java
    public static void main(String[] args) {
        int[] nums = {5, 10, 15};
        doubleFirst(nums);
        System.out.println(nums[0]);
    }

    public static void doubleFirst(int[] ids) {
        ids[0] = ids[0] * 2;
    }
    ```

    2. Now trace this code. What does `main` print?
    ```java
    public static void main(String[] args) {
        int x = 5;
        tryToChange(x);
        System.out.println(x);
    }

    public static void tryToChange(int n) {
        n = n * 2;
    }
    ```

    3. Problems 1 and 2 look similar but behave differently. Explain the difference in one or two sentences. Use the terms *copy* and *reference*.

    4. Trace this code fully. What does `main` print after `addOne` is called?
    ```java
    public static void main(String[] args) {
        int[] data = {10, 20, 30};
        addOne(data);
        for (int i = 0; i < data.length; i++) {
            System.out.print(data[i] + " ");
        }
    }

    public static void addOne(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            nums[i] += 1;
        }
    }
    ```

    5. True or false — explain your answer.
       <br>a) A method can permanently change the contents of an array it receives as a parameter.
       <br> b) A method can permanently change an `int` variable it receives as a parameter.

    ### Part 2: Methods That Take Arrays and Return Values

    All methods below should return a value — no printing inside the method.

    6. Write a method `sum` that takes an `int` array and returns the sum of its elements.
    7. Write a method `average` that takes an `int` array and returns the average as a `double`.

    ### Part 3: Putting It Together

    8. Using your `sum` and `average` methods from Part 2, write the `main` code (not the methods) that declares the array `{88, 72, 95, 61, 83}`, prints the sum, and prints the average.

    ### Part 4: Find the Bug

    9. Find the bug. 
    ```java
    public static int sum(int[] arr) {
        int total = 0;
        for (int i = 0; i <= arr.length; i++) {
            total += arr[i];
        }
        return total;
    }
    ```

    10. Compiles and runs — but returns the wrong answer for most inputs. What is wrong?
    ```java
    public static double average(int[] arr) {
        int total = 0;
        for (int i = 0; i < arr.length; i++) {
            total += arr[i];
        }
        return total / arr.length;
    }
    ```
