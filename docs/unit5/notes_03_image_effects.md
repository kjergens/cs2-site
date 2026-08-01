# CS2 — Unit Notes: Altering an Image

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
