# CS2 — Unit Notes: Traversing Images with Nested Loops

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
