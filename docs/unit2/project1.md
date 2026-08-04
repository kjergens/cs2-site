# Project 1: Arrays & Methods

**CS2 — Ms. Jergens**

---

## Overview

For this project you will choose one of the two options below. Both options require you to use arrays and methods — the two main topics of this unit. Choose the option that interests you more; your project will be better if you're engaged with the content.

---

## Option A: Text Analyzer

Pick a song, poem, or short passage that means something to you. You will write a Java program that stores the text and analyzes it in several ways.

**Requirements:**
- Store the text as a `String` array. Each element should be one line (or one word, your choice — just be consistent).
- Write at least 4 methods, of which at least 3 must return a value. Suggested methods:
    - Count how many lines/words contain a specific word or character (returns `int`)
    - Find and return the longest line or word (returns `String`)
    - Count and return the total number of words across all lines (returns `int`)
    - Print a formatted version of the text (can be void)
- Call all your methods from `main` and print the results with clear labels.
- Use a loop to traverse the array in at least two of your methods.

**Example output:**
```
Text: "Bohemian Rhapsody" by Queen
Total lines: 12
Total words: 87
Longest line: "Is this the real life? Is this just fantasy?"
Lines containing "me": 4
```

---

## Option B: Choose Your Own Adventure

Write a text-based adventure game where the player makes choices that affect the story. The story should be original and personal — set it somewhere you know, use characters you invent, make it yours.

**Requirements:**
- Store at least one set of options or story elements in a `String` array (e.g., room descriptions, item names, or choice menus).
- Write at least 4 methods, of which at least 2 must return a value. Suggested methods:
    - A method that displays a scene and returns the player's choice (returns `String` or `int`)
    - A method that checks whether a choice is valid (returns `boolean`)
    - A method that prints a formatted header or menu (can be void)
    - A method for at least one major scene in your story
- The game must have at least 3 scenes and at least 2 decision points where the player's input changes what happens next.
- Use a loop somewhere in your program (e.g., to re-prompt the player for a valid input, or to display a menu).

**Example interaction:**
```
Welcome to the Haunted Dorm.
You are standing in the hallway.

What do you do?
1. Open the door on the left
2. Open the door on the right
3. Run back outside

Enter 1, 2, or 3:
```

---

## Grading Rubric

| Requirement | Points |
|---|---|
| Program runs without errors | 15 |
| Array is used correctly (declared, initialized, accessed with a loop) | 15 |
| At least 4 methods written; at least 2–3 return a value | 25 |
| Methods are called from `main` and results are used/printed | 15 |
| Personal content — your own song, story, or characters | 10 |
| Code is readable: meaningful variable/method names, consistent indentation | 10 |
| Written reflection (see below) | 10 |
| **Total** | **100** |

---

## Written Reflection (10 pts)

At the top of your code as a comment block, answer the following:

- Which option did you choose, and why?
- What was the hardest part of this project?
- What would you add if you had more time?

---

## Timeline

| Class | Goal |
|---|---|
| 1 | Choose your option. Plan your array and method list on paper. Start coding. |
| 2 | Methods drafted. Array working. |
| 3 | All requirements met. Testing and debugging. |
| 4 | Final polish. Submit. |

**Submission:** Upload your `.java` file to Schoology by the end of Class 4.
