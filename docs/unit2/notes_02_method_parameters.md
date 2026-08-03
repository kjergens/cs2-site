# CS2 — Unit Notes: Method Parameters and Local Scope

---

## 1. Parameters Are Copies

When you pass a **primitive** value (int, double, boolean, etc.) to a method, Java gives the method its own **copy** of that value. Changing the copy inside the method has no effect on the original variable in the caller.

```java
public static void addFive(int n) {
    n = n + 5;
    System.out.println("Inside method: n = " + n);
}

public static void main(String[] args) {
    int x = 10;
    addFive(x);
    System.out.println(x);   // still 10
}
```

Output:
```
Inside method: n = 15
10
```

`x` is still 10 because `n` is a separate variable that holds a copy of `x`. When `addFive` changes `n`, it is only changing its own copy.

**The rule:** primitives are passed **by value** — the method gets a copy, not the original.

---

## 2. Multiple Parameters

A method can take multiple parameters, separated by commas. Each parameter has its own type and name.

```java
public static void printMultiples(int n, int count) {
    for (int i = 1; i <= count; i++) {
        System.out.println(n * i);
    }
}
```

**Calling it:**
```java
printMultiples(3, 4);   // prints 3, 6, 9, 12
printMultiples(7, 3);   // prints 7, 14, 21
```

**Order matters:** the first argument maps to the first parameter, the second to the second. `printMultiples(4, 3)` and `printMultiples(3, 4)` produce different output.

---

## 3. Local Scope

A variable declared inside a method **only exists inside that method**. It is created when the method is called and destroyed when the method returns. This is called **local scope**.

```java
public static void compute() {
    int result = 42;        // local to compute()
    System.out.println(result);
}

public static void main(String[] args) {
    compute();
    System.out.println(result);   // COMPILE ERROR — result doesn't exist here
}
```

Parameters are also local — they behave like variables declared at the top of the method.

**Two methods can use the same variable name with no conflict:**

```java
public static void methodA() {
    int count = 0;
    count++;
    System.out.println("A: " + count);   // A: 1
}

public static void methodB() {
    int count = 100;
    count--;
    System.out.println("B: " + count);   // B: 99
}
```

Each `count` is a separate variable that only exists while its method is running. They do not interfere.

---

## 4. Putting It Together: Void Methods with Parameters

A void method that uses parameters and local variables:

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

- `size` is a parameter (set by the caller)
- `row` and `col` are local variables (only exist during the loop)
- Nothing is returned — `void` does the action directly

---

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `System.out.println(result)` in main after `result` declared in another method | Compile error: variable not in scope | Variables are local; they can't escape their method |
| Expecting a primitive to change after being passed to a method | Logic error | Primitives are copied; the original is unchanged |
| `printMultiples(4)` when method needs two params | Compile error | Supply all required arguments |
| Declaring a variable with the same name as a parameter | Compile error | The parameter already declares that name in this scope |

---

## Check Your Understanding

### Part A: Concepts

**1.** What does "passed by value" mean for primitives?

**2.** After this runs, what does `main` print for `x`?
```java
public static void triple(int n) { n = n * 3; }
public static void main(String[] args) {
    int x = 5;
    triple(x);
    System.out.println(x);
}
```

**3.** Will this compile? Why or why not?
```java
public static void setup() { int total = 0; }
public static void main(String[] args) { System.out.println(total); }
```

---

### Part B: Predict the Output

**4.**
```java
public static void mystery(int a, int b) {
    a = a * 2;
    b = b + 3;
    System.out.println(a + " " + b);
}
public static void main(String[] args) {
    int p = 4;
    int q = 7;
    mystery(p, q);
    System.out.println(p + " " + q);
}
```

**5.**
```java
public static void methodA() {
    int count = 0;
    count++;
    System.out.println("A: " + count);
}
public static void methodB() {
    int count = 100;
    count--;
    System.out.println("B: " + count);
}
public static void main(String[] args) {
    methodA();
    methodB();
    methodA();
}
```

---

### Part C: Write the Code

**6.** Write a void method `printMultiples(int n, int count)` that prints the first `count` multiples of `n`, one per line.

---
---

## Answer Key

### Part A

**1.** The method receives its own copy of the value. Changing the copy inside the method does not affect the original variable in the caller.

**2.** `5` — `triple` changes its local copy of `n`, not the original `x`.

**3.** No. `total` is local to `setup()` and doesn't exist in `main`.

### Part B

**4.**
```
8 10
4 7
```
Inside `mystery`: a=8, b=10 (copies of p and q modified). Back in main: p and q unchanged.

**5.**
```
A: 1
B: 99
A: 1
```
Each call creates its own `count`. They don't accumulate.

### Part C

**6.**
```java
public static void printMultiples(int n, int count) {
    for (int i = 1; i <= count; i++) {
        System.out.println(n * i);
    }
}
```

---

## Homework 6: Method Parameters

!!! attention

    **Unit 2 · Chapter 2**

    *Assigned Class 7 · Due Class 8*

    ### Part 1: Parameters — Copies, Not Connections

    1. Trace through this code. Fill in the value of `x` in `main` after each line.
    ```java
    public static void main(String[] args) {
        int x = 10;                  // x = ____
        addFive(x);                  // prints: ________________
        System.out.println(x);       // prints: ________________
    }

    public static void addFive(int n) {
        n = n + 5;
        System.out.println("Inside method: n = " + n);
    }
    ```
    2. Why doesn't `x` change after `addFive(x)` is called? Explain in one or two sentences.
    3. Predict the output of this program:
    ```java
    public static void main(String[] args) {
        int p = 4;
        int q = 7;
        mystery(p, q);
        System.out.println(p + " " + q);
    }

    public static void mystery(int a, int b) {
        a = a * 2;
        b = b + 3;
        System.out.println(a + " " + b);
    }
    ```

    ### Part 2: Local Scope

    4. Will this code compile? Explain why or why not.
    ```java
    public static void main(String[] args) {
        compute();
        System.out.println(result);
    }

    public static void compute() {
        int result = 42;
    }
    ```
    5. Two methods each declare a variable named `count`. Do they interfere with each other? Why or why not?
    ```java
    public static void main(String[] args) {
        methodA();
        methodB();
        methodA();
    }

    public static void methodA() {
        int count = 0;
        count++;
        System.out.println("A: " + count);
    }

    public static void methodB() {
        int count = 100;
        count--;
        System.out.println("B: " + count);
    }
    ```

    ### Part 3: Write the Method

    6. Write a void method called `printMultiples` that takes two `int` parameters — a number `n` and a count `k` — and prints the first `k` multiples of `n`, each on its own line. Example: `printMultiples(3, 4)` should print `3 6 9 12`, one per line.

    7. Write a void method called `printBox` that takes an `int size` and prints a filled square of `*` characters of that size. Example: `printBox(3)` should print a 3×3 grid of `* * *` rows.
