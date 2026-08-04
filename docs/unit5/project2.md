# Project 2: FotoFun — Object-Oriented Image Effects

**CS2 — Ms. Jergens**

---

## Overview

In this project you will write a Java class called `FotoFun` that analyzes an image, draws a pattern on it, and applies visual effects. You will practice designing a class with private attributes and a constructor — and apply what you know about RGB color values and pixel manipulation.

Starter code is provided that loads your image and displays the result. Your job is to complete the `FotoFun` class.

---

## The FotoFun Class

Your class must have:

- Two private attributes: one to hold the image, one to hold the image's title (a `String` of your choice)
- A constructor that takes the image and a title as parameters and sets both attributes
- A getter for the title
- Five methods (see below)

**Example usage** (in the provided starter code):
```java
FotoFun foto = new FotoFun(image, "My Cat Mochi");
foto.analyze();
foto.drawPattern();
foto.transform1();
foto.transform2();
foto.myEffect();
```

---

## Method 1: `analyze` (required)

**Signature:** `public void analyze()`

Loop through every pixel in the image. Calculate and print the percentage of red, green, and blue across all pixels.

**Example output:**
```
Title: My Cat Mochi
Red:   38.2%
Green: 29.7%
Blue:  32.1%
```

---

## Method 2: `drawPattern` (pick one)

**Signature:** `public void drawPattern()`

Draw a pattern directly on top of the image by setting certain pixels to a color of your choice. Pick one of the following:

| Option | Description |
|---|---|
| Checkerboard | Divide the image into squares (e.g., 20×20 px). Color alternating squares. |
| Stripes | Draw evenly spaced horizontal or vertical stripes across the image. |
| Border | Draw a solid-color border around the edges of the image. |
| Starburst | Draw lines radiating outward from the center of the image. |

---

## Method 3 & 4: `transform1` and `transform2` (both required)

**Signatures:** `public void transform1()` and `public void transform2()`

Implement both of the following. Each method modifies every pixel based on its original RGB values.

| Method | Description |
|---|---|
| Grayscale | Set R, G, B all to their average: `(r + g + b) / 3` |
| Invert | Each channel becomes `255 − original` |

---

## Method 5: `myEffect` (your choice)

**Signature:** `public void myEffect()`

Implement one effect of your choice. Pick from the list below, or propose your own (ask first).

| Option | Description |
|---|---|
| Brighten | Increase each RGB value by a fixed amount (cap at 255) |
| Shift Colors | Cycle the channels: R→G, G→B, B→R |
| Black & White | Each pixel becomes pure black or pure white based on brightness |
| Sepia | Apply a warm orange-brown tint for an old-fashioned look |
| Mirror | Copy the left half of the image onto the right half (flipped) |
| Shrink | Output a version at half the width and height |

---

## Grading Rubric

| Requirement | Points |
|---|---|
| Program runs without errors | 10 |
| Class design: private attributes, constructor, getter | 20 |
| `analyze()` correctly calculates and prints RGB percentages | 15 |
| `drawPattern()` draws a visible, correct pattern | 15 |
| `transform1()` and `transform2()` both work correctly | 25 |
| `myEffect()` works correctly and is visibly distinct | 10 |
| Code quality: meaningful names, consistent indentation | 5 |
| **Total** | **100** |

---

## Written Reflection

At the top of your `FotoFun.java` file, in a comment block, answer:

- Which pattern and custom effect did you choose, and why?
- What was the hardest part of this project?
- What would you add if you had more time?

---

## Timeline

| Class | Goal |
|---|---|
| 1 | Constructor and `analyze()` working. Pattern chosen and started. |
| 2 | `drawPattern()` done. Both transforms working. |
| 3 | `myEffect()` done. All five methods complete. |
| 4 | Testing, debugging, reflection written. Submit. |

**Submission:** Upload `FotoFun.java` to Schoology by the end of Class 4.
