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

---

## Homework 19: ArrayLists and Loops

*Assigned Class 38 · Due Class 39*

An ArrayList becomes much more powerful once you combine it with loops — you can search, count, filter, and compute across every element. This homework uses an ArrayList of a custom object type, so you'll also practice calling methods on objects you retrieve from the list.

The following `Student` class is used throughout:
```java
public class Student {
    private String name;
    private double gpa;

    public Student(String name, double gpa) {
        this.name = name;
        this.gpa = gpa;
    }

    public String getName() { return name; }
    public double getGpa()  { return gpa; }

    public String toString() {
        return name + " (GPA: " + gpa + ")";
    }
}
```

### Part 1: Iterating an ArrayList

Two ways to loop through an `ArrayList<Student>`:
```java
// Index-based
for (int i = 0; i < roster.size(); i++) {
    Student s = roster.get(i);
    System.out.println(s.getName());
}

// For-each
for (Student s : roster) {
    System.out.println(s.getName());
}
```

1. An ArrayList uses `.size()` to get its length and `.get(i)` to retrieve an element. How are these different from what you use with a regular array?
2. Both loops above produce identical output. Name one thing you can do with the index-based loop that you cannot do with the for-each loop.
3. Describe step by step what `roster.get(1).getGpa()` does.
4. `roster` contains 5 students. What is the last valid index? What happens if you call `roster.get(5)`?

### Part 2: Predict the Output

5.
```java
ArrayList<Student> roster = new ArrayList<>();
roster.add(new Student("Alex",   3.8));
roster.add(new Student("Jordan", 2.9));
roster.add(new Student("Casey",  3.5));

for (Student s : roster) {
    System.out.println(s);
}
System.out.println("Size: " + roster.size());
```

6.
```java
ArrayList<Student> roster = new ArrayList<>();
roster.add(new Student("Alex",   3.8));
roster.add(new Student("Jordan", 2.9));
roster.add(new Student("Casey",  3.5));
roster.add(new Student("Morgan", 3.1));

int count = 0;
for (Student s : roster) {
    if (s.getGpa() >= 3.5) {
        count++;
    }
}
System.out.println(count);
```

7.
```java
ArrayList<Student> roster = new ArrayList<>();
roster.add(new Student("Alex",   3.8));
roster.add(new Student("Jordan", 2.9));
roster.add(new Student("Casey",  3.5));

Student best = roster.get(0);
for (int i = 1; i < roster.size(); i++) {
    if (roster.get(i).getGpa() > best.getGpa()) {
        best = roster.get(i);
    }
}
System.out.println(best.getName());
```

### Part 3: Write Methods

8. Write a method `countHonorRoll` that takes an `ArrayList<Student>` and returns how many students have a GPA of 3.5 or higher.

9. Write a method `topStudent` that takes an `ArrayList<Student>` and returns the `Student` with the highest GPA. Assume the list has at least one element.

10. Write a method `getNames` that takes an `ArrayList<Student>` and returns a new `ArrayList<String>` containing each student's name, in the same order.

### Part 4: Find the Bug

11.
```java
public static void printAll(ArrayList<Student> roster) {
    for (int i = 0; i <= roster.size(); i++) {
        System.out.println(roster.get(i));
    }
}
```

12.
```java
public static int countHonorRoll(ArrayList<Student> roster) {
    int count = 0;
    for (Student s : roster) {
        if (s.getGpa() >= 3.5) {
            count++;
        }
    }
    return count;
}

public static void main(String[] args) {
    ArrayList<Student> roster = new ArrayList<>();
    roster.add(new Student("Alex", 3.8));
    System.out.println(roster.length);
}
```

13. Compiles and runs — but may return the wrong answer. What is the problem?
```java
public static boolean contains(ArrayList<Student> roster, String targetName) {
    for (Student s : roster) {
        if (s.getName() == targetName) {
            return true;
        }
    }
    return false;
}
```
