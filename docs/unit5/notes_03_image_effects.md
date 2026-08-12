# CS2 — Unit 3 Chapter 3: Altering an Image

---

## 1. The Read-Modify-Write Pattern

Every image effect follows the same three-step pattern:

1. **Read** the pixel's current color
2. **Modify** the channel values
3. **Write** a new color back to that pixel

```java
for (int y = 0; y < image.getHeight(); y++) {
    for (int x = 0; x < image.getWidth(); x++) {
        // 1. Read
        Color pixel = new Color(image.getRGB(x, y));
        int r = pixel.getRed();
        int g = pixel.getGreen();
        int b = pixel.getBlue();

        // 2. Modify — compute new values
        int newR = r + 50;
        int newG = g + 50;
        int newB = b + 50;

        // 3. Write — create a new Color and set it
        Color newPixel = new Color(newR, newG, newB);
        image.setRGB(x, y, newPixel.getRGB());
    }
}
```

The step in the middle changes depending on which effect you're implementing. The read and write steps are always the same.

---

## 2. Clamping Values to [0, 255]

Modifying channel values can push them outside the valid range (0–255). For example, `r + 50` on a pixel where `r = 220` gives `270`, which throws an exception when passed to `new Color(...)`.

The fix is **clamping** — forcing a value into the valid range:

```java
public static int clamp(int value) {
    return Math.min(255, Math.max(0, value));
}
```

Usage:
```java
int newR = clamp(r + 50);   // safely limits to [0, 255]
int newG = clamp(g + 50);
int newB = clamp(b + 50);
```

Always clamp before creating a new `Color`. This is equivalent to the `clamp` method from Unit 2 — same algorithm, new context.

---

## 3. Brightness

Increase or decrease all three channels by the same amount:

```java
int amount = 50;   // positive = brighter, negative = darker
int newR = clamp(r + amount);
int newG = clamp(g + amount);
int newB = clamp(b + amount);
```

---

## 4. Grayscale

Replace each pixel's color with a gray of the same overall brightness. The standard approach averages the three channels:

```java
int avg = (r + g + b) / 3;
int newR = avg;
int newG = avg;
int newB = avg;
```

The result is always a shade of gray (equal R, G, B). For better perceptual accuracy, a weighted average is used in practice (`0.299*r + 0.587*g + 0.114*b`), but the simple average is fine for this course.

---

## 5. Red Filter (Isolating a Channel)

Keep one channel, zero out the others:

```java
// Red filter — keep red, remove green and blue
int newR = r;
int newG = 0;
int newB = 0;
```

---

## 6. Negative / Invert

Subtract each channel from 255:

```java
int newR = 255 - r;
int newG = 255 - g;
int newB = 255 - b;
```

No clamping needed here — if `r` is 0–255, then `255 - r` is always 0–255.

---

## 7. A Complete Effect Method

The pattern for a reusable effect method:

```java
public static void brighten(BufferedImage image, int amount) {
    for (int y = 0; y < image.getHeight(); y++) {
        for (int x = 0; x < image.getWidth(); x++) {
            Color pixel = new Color(image.getRGB(x, y));
            int r = pixel.getRed();
            int g = pixel.getGreen();
            int b = pixel.getBlue();

            int newR = clamp(r + amount);
            int newG = clamp(g + amount);
            int newB = clamp(b + amount);

            image.setRGB(x, y, new Color(newR, newG, newB).getRGB());
        }
    }
}
```

The method modifies the image in place (void) — this is the array-as-parameter pattern from Unit 2 applied to image objects.

---

## 8. Common Errors

| Error | Problem | Fix |
|---|---|---|
| `new Color(r + 50, g + 50, b + 50)` without clamping | Exception if any value > 255 or < 0 | Always clamp: `clamp(r + 50)` |
| Forgetting `image.setRGB(x, y, ...)` | Pixels read and modified but never written back | The write step is required |
| `image.getRGB(x, y)` after modification shows old value | `getRGB` returns the stored int — must re-wrap in `Color` | Always wrap in `new Color(...)` |

---

## Check Your Understanding

!!! information

    **Unit 5 · Chapter 3**

    **1.** What does `clamp(300)` return? What does `clamp(-10)` return?

    **2.** Write the modify step for a grayscale effect using the average of R, G, B.

    **3.** Write the modify step for a "warm" filter that adds 30 to red and subtracts 30 from blue (clamped).

    **4.** If a pixel has R=200, G=150, B=100, what are the new R, G, B values after a brightness increase of 80?

    ---
    ---

    ## Answer Key

    **1.** `clamp(300)` → 255. `clamp(-10)` → 0.

    **2.**
    ```java
    int avg  = (r + g + b) / 3;
    int newR = avg;
    int newG = avg;
    int newB = avg;
    ```

    **3.**
    ```java
    int newR = clamp(r + 30);
    int newG = g;
    int newB = clamp(b - 30);
    ```

    **4.** R: clamp(200+80) = clamp(280) = 255. G: clamp(150+80) = clamp(230) = 230. B: clamp(100+80) = 180.

---

## Homework 17: Image Transformation — Brightness

!!! attention

    **Unit 5 · Chapter 3**

    *Assigned Class 32 · Due Class 33*

    ### Part 1: The Read-Modify-Write Pattern

    Here is a method that sets every pixel to pure red, ignoring the original colors:

    ```java
    public static void makeRed(BufferedImage img) {
        for (int y = 0; y < img.getHeight(); y++) {
            for (int x = 0; x < img.getWidth(); x++) {
                Color newColor = new Color(255, 0, 0);
                img.setRGB(x, y, newColor.getRGB());
            }
        }
    }
    ```

    1. What does `img.setRGB(x, y, newColor.getRGB())` do? Why do we call `.getRGB()` on the `Color` object instead of passing `newColor` directly?
    2. `makeRed` ignores the original pixel entirely. Rewrite the inner loop body so it reads the original pixel, keeps its green and blue values, but replaces the red channel with 255.
    3. `makeRed` is void — it returns nothing. Yet after calling `makeRed(img)`, the image is permanently changed. Why? (Think back to HW 7.)
    4. You have a pixel at (3, 5) with color (80, 120, 200). Write the single line of code that sets that pixel to (80, 120, 255) — same red and green, but full blue.

    ### Part 2: Clamping

    Each RGB channel must stay in the range [0, 255]. Adding or subtracting a fixed amount can push a value outside that range, so you must clamp the result.

    5. A pixel has red = 220. You add 50 to brighten it. a) What is 220 + 50? b) Is that a valid channel value? What should be stored instead?
    6. A pixel has blue = 20. You subtract 50 to darken it. a) What is 20 − 50? b) Is that valid? What should be stored instead?
    7. Write a helper method `clamp(int value)` that returns `value` clamped to [0, 255]: if below 0, return 0; if above 255, return 255; otherwise return the value unchanged.

    ### Part 3: Write the Transformation

    8. Using your `clamp` method, write `brighten(BufferedImage img, int amount)`. It should add `amount` to each channel (red, green, blue) of every pixel, clamping each result to [0, 255]. The method modifies the image in place.

    9. Trace `brighten` on this 3-pixel image (1 row, 3 columns) with `amount = 40`. Fill in the new RGB values after brightening.

    | Pixel | Original (R, G, B) | New (R, G, B) |
    |---|---|---|
    | (0, 0) | (100, 150, 200) | |
    | (1, 0) | (220, 30, 180) | |
    | (2, 0) | (50, 80, 10) | |

    10. What happens when you call `brighten(img, -30)`? What visual effect does that produce, and does `clamp` still protect against invalid values?

    ### Part 4: Find the Bug

    11. Find the bug.
    ```java
    public static void brighten(BufferedImage img, int amount) {
        for (int y = 0; y < img.getHeight(); y++) {
            for (int x = 0; x < img.getWidth(); x++) {
                Color c = new Color(img.getRGB(x, y));
                int r = clamp(c.getRed()   + amount);
                int g = clamp(c.getGreen() + amount);
                int b = clamp(c.getBlue()  + amount);
                Color newColor = new Color(r, g, b);
            }
        }
    }
    ```

    12. What goes wrong when `amount` is large enough to push a channel past 255, or negative enough to push it below 0?
    ```java
    public static void brighten(BufferedImage img, int amount) {
        for (int y = 0; y < img.getHeight(); y++) {
            for (int x = 0; x < img.getWidth(); x++) {
                Color c = new Color(img.getRGB(x, y));
                int r = c.getRed()   + amount;
                int g = c.getGreen() + amount;
                int b = c.getBlue()  + amount;
                img.setRGB(x, y, new Color(r, g, b).getRGB());
            }
        }
    }
    ```

    13. Find the bug.
    ```java
    public static void brighten(BufferedImage img, int amount) {
        for (int y = 0; y < img.getHeight(); y++) {
            for (int x = 0; x < img.getWidth(); x++) {
                Color c = new Color(img.getRGB(x, y));
                int r = clamp(c.getRed()   + amount);
                int g = clamp(c.getGreen() + amount);
                int b = clamp(c.getBlue()  + amount);
                img.setRGB(y, x, new Color(r, g, b).getRGB());
            }
        }
    }
    ```
