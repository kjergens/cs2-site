# CS2 — Unit Notes: ArrayList Traversal and Algorithms

---

## 1. Traversal with a Standard For Loop

The standard `for` loop over an ArrayList looks almost identical to array traversal — replace `arr.length` with `list.size()` and `arr[i]` with `list.get(i)`:

```java
ArrayList<String> names = new ArrayList<>();
// ... names has been populated ...

for (int i = 0; i < names.size(); i++) {
    System.out.println(names.get(i));
}
```

Use the standard for loop when you need the index (to compare adjacent elements, to modify by position, or to skip certain indices).

---

## 2. Traversal with the Enhanced For Loop

The **enhanced for loop** (also called "for-each") is cleaner when you only need the value, not the position:

```java
for (String name : names) {
    System.out.println(name);
}
```

Read it as: "for each String `name` in `names`..."

The enhanced for loop:
- Is shorter and easier to read
- Cannot be used when you need the index or when you plan to modify the list

---

## 3. Standard vs. Enhanced For — When to Use Each

| Situation | Use |
|---|---|
| Just reading values | Enhanced for |
| Need the index to do something | Standard for |
| Modifying elements with `set(i, ...)` | Standard for |
| Removing elements while iterating | Neither — see below |

---

## 4. Common Patterns

### Find (return the first match)

```java
public static String findFirst(ArrayList<String> list, String target) {
    for (String item : list) {
        if (item.equals(target)) {
            return item;
        }
    }
    return null;   // not found
}
```

### Count (how many meet a condition)

```java
public static int countAbove(ArrayList<Integer> scores, int threshold) {
    int count = 0;
    for (int score : scores) {
        if (score > threshold) {
            count++;
        }
    }
    return count;
}
```

### Collect (build a new list from matches)

```java
public static ArrayList<String> namesStartingWith(ArrayList<String> names, char letter) {
    ArrayList<String> result = new ArrayList<>();
    for (String name : names) {
        if (name.charAt(0) == letter) {
            result.add(name);
        }
    }
    return result;
}
```

### Max / Min

```java
public static int max(ArrayList<Integer> list) {
    int maxVal = list.get(0);
    for (int val : list) {
        if (val > maxVal) {
            maxVal = val;
        }
    }
    return maxVal;
}
```

---

## 5. Removing Elements While Traversing

Removing from an ArrayList inside a forward for loop causes elements to shift and skip. The safe approaches:

**Iterate backwards** (works for removing by condition):

```java
for (int i = list.size() - 1; i >= 0; i--) {
    if (list.get(i) < 0) {
        list.remove(i);
    }
}
```

When you remove at index `i` going backwards, the elements above `i` shift left — but those have already been visited, so nothing is skipped.

---

## 6. ArrayList of Objects

ArrayLists can hold any object type:

```java
ArrayList<BankAccount> accounts = new ArrayList<>();
accounts.add(new BankAccount("Alice", 500.0));
accounts.add(new BankAccount("Bob", 200.0));

for (BankAccount acct : accounts) {
    System.out.println(acct);   // calls toString()
}
```

The unit notes from Units 3–4 apply: each element in the list is an object with its own state and methods.

---

## 7. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `for (int i = 0; i < list.size(); i++) { list.remove(i); }` | Elements shift during removal; some are skipped | Iterate backwards or collect indices first |
| `list.get(list.size())` | size() returns count; last valid index is size()-1 | `list.get(list.size() - 1)` |
| `for (String s : list) { list.remove(s); }` | Modifying list during enhanced for causes `ConcurrentModificationException` | Use backwards index loop for removal |

---

## Check Your Understanding

**1.** Write a for-each loop that prints every element in `ArrayList<Integer> scores`.

**2.** Write a method `public static int countEvens(ArrayList<Integer> list)` that returns the number of even values.

**3.** Given `["cat", "dog", "ant", "bee"]`, what does this return?
```java
ArrayList<String> result = namesStartingWith(list, 'a');
```

**4.** Why is it unsafe to remove elements from an ArrayList using a forward for loop?

---
---

## Answer Key

**1.**
```java
for (int score : scores) {
    System.out.println(score);
}
```

**2.**
```java
public static int countEvens(ArrayList<Integer> list) {
    int count = 0;
    for (int val : list) {
        if (val % 2 == 0) count++;
    }
    return count;
}
```

**3.** `["ant"]` — only "ant" starts with 'a'.

**4.** When you remove an element at index `i`, the element that was at `i+1` shifts to `i`. The loop then increments `i` to `i+1`, skipping the shifted element. Elements get skipped and may not be checked.
