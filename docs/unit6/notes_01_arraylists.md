# CS2 — Unit Notes: ArrayLists

---

## 1. Why ArrayList?

Arrays are fixed in size. Once you declare `int[] scores = new int[30]`, it holds exactly 30 elements — no more, no less. If you need to add or remove elements, you're stuck manually shifting values or creating a new array.

An `ArrayList` solves this. It grows and shrinks automatically as you add and remove elements.

| Feature | Array | ArrayList |
|---|---|---|
| Size | Fixed at declaration | Grows/shrinks dynamically |
| Adding elements | Not supported (must resize manually) | `add(item)` |
| Removing elements | Not supported directly | `remove(index)` |
| Accessing elements | `arr[i]` | `list.get(i)` |
| Length/size | `arr.length` | `list.size()` |
| Works with primitives | Yes (`int[]`) | No — must use wrapper types (`Integer`) |

---

## 2. Creating an ArrayList

You must import ArrayList before using it:

```java
import java.util.ArrayList;   // specific import
// or
import java.util.*;           // wildcard — imports everything in java.util
```

Declaring and creating:

```java
ArrayList<String>  names  = new ArrayList<>();
ArrayList<Integer> scores = new ArrayList<>();
ArrayList<Double>  gpas   = new ArrayList<>();
```

The type inside `< >` is the **type parameter** — it tells Java what kind of objects the list holds. You can't use primitive types here: use `Integer` instead of `int`, `Double` instead of `double`.

---

## 3. Core Methods

### `add(item)` — append to the end

```java
ArrayList<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.add("Carol");
// names is now ["Alice", "Bob", "Carol"]
```

There's also `add(index, item)` — inserts at a position and shifts everything right.

### `get(index)` — access by position (0-indexed)

```java
String first = names.get(0);   // "Alice"
String last  = names.get(names.size() - 1);  // "Carol"
```

### `size()` — number of elements

```java
int n = names.size();   // 3
```

Note: `size()` is a method (with parentheses), unlike array's `length` field.

### `remove(index)` — remove by position

```java
names.remove(1);   // removes "Bob"; list is now ["Alice", "Carol"]
```

After removal, elements shift left — the size decreases by 1.

### `remove(Object)` — remove by value

```java
names.remove("Alice");   // removes first occurrence of "Alice"
```

For `ArrayList<Integer>`, `remove(0)` removes by index, but `remove(Integer.valueOf(5))` removes by value. Be careful with the distinction.

### `set(index, item)` — replace at position

```java
names.set(0, "Alicia");   // ["Alicia", "Carol"]
```

---

## 4. Printing an ArrayList

Unlike arrays (which print as a memory address), `ArrayList` has a built-in `toString`:

```java
System.out.println(names);   // [Alice, Bob, Carol]
```

---

## 5. A Complete Example

```java
import java.util.ArrayList;

ArrayList<Integer> scores = new ArrayList<>();
scores.add(85);
scores.add(92);
scores.add(78);
scores.add(90);

System.out.println(scores);          // [85, 92, 78, 90]
System.out.println(scores.size());   // 4
System.out.println(scores.get(1));   // 92

scores.remove(0);                    // removes 85
System.out.println(scores);          // [92, 78, 90]

scores.set(1, 95);
System.out.println(scores);          // [92, 95, 90]
```

---

## 6. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `ArrayList<int>` | Can't use primitives as type parameters | Use `ArrayList<Integer>` |
| `list.length` | Arrays use `length`; ArrayList uses `size()` | `list.size()` |
| `list[i]` | Arrays use bracket notation; ArrayList uses a method | `list.get(i)` |
| `IndexOutOfBoundsException` on `get(i)` | `i >= list.size()` | Check index against `list.size() - 1` |
| Forgetting the import | Compile error: "cannot find symbol" | Add `import java.util.ArrayList;` |

---

## Check Your Understanding

**1.** What import statement do you need to use ArrayList?

**2.** Create an `ArrayList<String>` called `fruits`, add "apple", "banana", and "cherry" to it, then print the second element.

**3.** What is the difference between `list.length` and `list.size()`?

**4.** After these operations, what does `list` contain?
```java
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.add(30);
list.remove(0);
list.set(0, 25);
System.out.println(list);
```

---
---

## Answer Key

**1.** `import java.util.ArrayList;` (or `import java.util.*;`)

**2.**
```java
ArrayList<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");
System.out.println(fruits.get(1));   // banana
```

**3.** `list.length` doesn't exist on ArrayList — that's array syntax. ArrayList uses `list.size()`.

**4.** `[25, 30]`. Trace: add 10→[10], add 20→[10,20], add 30→[10,20,30], remove(0)→[20,30], set(0,25)→[25,30].

---

## Homework 18: ArrayLists

!!! attention

    1. True or False: You can add items to an ArrayList after it has been created.
    2. How do you get the first item from an ArrayList called `list`? Write the code.
    3. What does `list.add("dog")` do? (1 sentence)
    4. What is the index of the third item in any ArrayList?
    5. What does the following code print?
    ```java
    ArrayList<String> colors = new ArrayList<>();
    colors.add("green");
    colors.add("blue");
    colors.add(0, "red");
    System.out.println(colors.get(1));
    ```
    6. For the ArrayList in question 5, write the line of code to remove `"blue"`.
    7. What does the following code print?
    ```java
    ArrayList<String> food = new ArrayList<>();
    food.add("pizza");
    food.add("pineapple");
    food.add(1, "bread");
    System.out.println(food.get(0));
    ```
    8. What does the following code print (two lines)?
    ```java
    ArrayList<String> animals = new ArrayList<>();
    animals.add("cat");
    animals.add("dog");
    animals.add("fish");
    System.out.println(animals.size());
    animals.remove(1);
    System.out.println(animals.size());
    ```
    9. Short answer (1 sentence): Why would a programmer choose an ArrayList instead of a regular array?
