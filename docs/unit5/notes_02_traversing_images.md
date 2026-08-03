# CS2 — Unit 5 Chapter 2: Traversing Images with Nested Loops

---

## 1. The Traversal Pattern

An image is a 2D grid. To visit every pixel, you need two loops: one for the rows (y) and one for the columns (x).

```java
for (int y = 0; y < image.getHeight(); y++) {
    for (int x = 0; x < image.getWidth(); x++) {
        Color pixel = new Color(image.getRGB(x, y));
        // do something with pixel
    }
}
```

The outer loop controls the row (y — vertical position). The inner loop controls the column (x — horizontal position). For each (x, y) pair, you get the color at that pixel.

**Which loop is outer?** Either order works — (y outer, x inner) visits row by row; (x outer, y inner) visits column by column. Row-by-row is conventional and matches how humans read images (left to right, top to bottom).

---

## 2. Summing a Channel Across All Pixels

A common pattern: loop over every pixel, extract one channel, accumulate.

```java
// Total red across the entire image
long totalRed = 0;
for (int y = 0; y < image.getHeight(); y++) {
    for (int x = 0; x < image.getWidth(); x++) {
        Color pixel = new Color(image.getRGB(x, y));
        totalRed += pixel.getRed();
    }
}
int numPixels = image.getWidth() * image.getHeight();
double avgRed = (double) totalRed / numPixels;
System.out.println("Average red: " + avgRed);
```

Note: use `long` for the total, not `int` — a large image can have millions of pixels, and `255 × 1,000,000` overflows an `int`.

---

## 3. Finding the Dominant Color Channel

Which channel is strongest on average?

```java
public static String dominantColor(BufferedImage image) {
    long totalR = 0, totalG = 0, totalB = 0;

    for (int y = 0; y < image.getHeight(); y++) {
        for (int x = 0; x < image.getWidth(); x++) {
            Color pixel = new Color(image.getRGB(x, y));
            totalR += pixel.getRed();
            totalG += pixel.getGreen();
            totalB += pixel.getBlue();
        }
    }

    if (totalR >= totalG && totalR >= totalB) {
        return "red";
    } else if (totalG >= totalB) {
        return "green";
    } else {
        return "blue";
    }
}
```

This is an algorithm you've seen in Unit 1 (finding the max) applied to image data. The values being compared are channel totals instead of array elements, but the logic is identical.

---

## 4. Traversing a Region

You don't always need to visit every pixel. To work with just the top half:

```java
int halfHeight = image.getHeight() / 2;
for (int y = 0; y < halfHeight; y++) {
    for (int x = 0; x < image.getWidth(); x++) {
        // process only top half
    }
}
```

To work with a rectangular subregion from (x1, y1) to (x2, y2):
```java
for (int y = y1; y < y2; y++) {
    for (int x = x1; x < x2; x++) {
        // process only this region
    }
}
```

---

## 5. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `x < image.getHeight()` in inner loop | Swapped width/height; crashes on non-square images | x uses `getWidth()`, y uses `getHeight()` |
| `int totalRed` overflows on large images | 255 × millions of pixels exceeds int range | Use `long totalRed` |
| `image.getRGB(y, x)` — coordinates swapped | Reads wrong pixel | Argument order is `getRGB(x, y)`: column first, then row |

---

## Check Your Understanding

!!! information

    **Unit 5 · Chapter 2**

    **1.** An image is 400 × 300 pixels. How many pixels does a complete traversal visit?

    **2.** Rewrite the total-red code so it also computes the average green channel in the same loop.

    **3.** If `totalR = 10000`, `totalG = 8000`, `totalB = 12000`, what does `dominantColor` return?

    **4.** What loop bounds would you use to traverse only the right half of a 400-wide image?

    ---
    ---

    ## Answer Key

    **1.** 400 × 300 = 120,000 pixels.

    **2.**
    ```java
    long totalRed = 0, totalGreen = 0;
    for (int y = 0; y < image.getHeight(); y++) {
        for (int x = 0; x < image.getWidth(); x++) {
            Color pixel = new Color(image.getRGB(x, y));
            totalRed   += pixel.getRed();
            totalGreen += pixel.getGreen();
        }
    }
    int numPixels = image.getWidth() * image.getHeight();
    double avgRed   = (double) totalRed / numPixels;
    double avgGreen = (double) totalGreen / numPixels;
    ```

    **3.** `"blue"` — totalB (12000) is the largest.

    **4.** `for (int x = image.getWidth() / 2; x < image.getWidth(); x++)`

---

## Homework 16: Traversing a BufferedImage

!!! attention

    **Unit 5 · Chapter 2**

    *Assigned Class 30 · Due Class 31*

    ### Part 1: Traversal Structure

    ```java
    for (int y = 0; y < img.getHeight(); y++) {
        for (int x = 0; x < img.getWidth(); x++) {
            Color c = new Color(img.getRGB(x, y));
            // do something with c
        }
    }
    ```

    1. For an image that is 800 pixels wide and 600 pixels tall, how many total times does the inner loop body run?
    2. What does one execution of the inner loop body represent — what "thing" in the image does it correspond to?
    3. In what order are the pixels visited? Describe it in plain English (e.g. "left to right across the first row, then left to right across the second row...").
    4. The line `Color c = new Color(img.getRGB(x, y))` appears inside both loops. What would go wrong if you moved it to just before the outer loop (outside both loops)?

    ### Part 2: Tracing a Small Image

    The table below represents a 2×3 image (2 columns, 3 rows). Each cell shows the (red, green, blue) values of that pixel.

    | | x = 0 | x = 1 |
    |---|---|---|
    | **y = 0** | (200, 50, 30) | (180, 60, 20) |
    | **y = 1** | (150, 80, 40) | (190, 70, 35) |
    | **y = 2** | (160, 90, 25) | (170, 55, 30) |

    5. Trace this nested loop on the image above. Fill in the value of `totalRed` after each iteration of the inner loop (iterations 1–6, visiting `(0,0)`, `(1,0)`, `(0,1)`, `(1,1)`, `(0,2)`, `(1,2)` in order).
    ```java
    int totalRed = 0;
    for (int y = 0; y < img.getHeight(); y++) {
        for (int x = 0; x < img.getWidth(); x++) {
            Color c = new Color(img.getRGB(x, y));
            totalRed += c.getRed();
        }
    }
    ```
    6. Using the same image, compute `totalGreen` and `totalBlue` after the full traversal.
    7. Based on your totals from problems 5 and 6, which color is dominant in this image?

    ### Part 3: Write the Method

    8. Write a method `totalRed` that takes a `BufferedImage` and returns the sum of all pixels' red values as an `int`.

    9. Write a method `dominantColor` that takes a `BufferedImage` and returns `"red"`, `"green"`, or `"blue"` — whichever channel has the highest total across all pixels. If there's a tie, either answer is acceptable.

    10. Write a method `hasFullyRedPixel` that takes a `BufferedImage` and returns `true` if any pixel has red = 255, green = 0, and blue = 0 exactly, `false` otherwise. (Hint: return `true` as soon as you find one — you don't need to keep looping.)

    ### Part 4: Find the Bug

    11.
    ```java
    public static String dominantColor(BufferedImage img) {
        int totalRed = 0, totalGreen = 0, totalBlue = 0;
        for (int y = 0; y < img.getWidth(); y++) {
            for (int x = 0; x < img.getWidth(); x++) {
                Color c = new Color(img.getRGB(x, y));
                totalRed   += c.getRed();
                totalGreen += c.getGreen();
                totalBlue  += c.getBlue();
            }
        }
        // ... compare totals
    }
    ```

    12.
    ```java
    public static int totalGreen(BufferedImage img) {
        for (int y = 0; y < img.getHeight(); y++) {
            int total = 0;
            for (int x = 0; x < img.getWidth(); x++) {
                Color c = new Color(img.getRGB(x, y));
                total += c.getGreen();
            }
        }
        return total;
    }
    ```

    13. Correct result — but there's a performance problem. What is it, and how do you fix it?
    ```java
    public static boolean hasFullyRedPixel(BufferedImage img) {
        boolean found = false;
        for (int y = 0; y < img.getHeight(); y++) {
            for (int x = 0; x < img.getWidth(); x++) {
                Color c = new Color(img.getRGB(x, y));
                if (c.getRed() == 255 && c.getGreen() == 0 && c.getBlue() == 0) {
                    found = true;
                }
            }
        }
        return found;
    }
    ```
