# Project 1: Script Parser

## Project Overview
For this project, you will create a text analysis program and use it to analyze the script of Star Wars (1977).

<table style="border: none; border-collapse: collapse; width: 100%;">
  <tr style="border: none;">
    <td style="border: none; padding: 10px; text-align: center; width: 50%;">
      <img src="../images/starwarsscript.png)" alt="Description 1" style="width: 100%; max-width: 250px;">
    </td>
    <td style="border: none; padding: 10px; text-align: center; width: 50%;">
      <img src="../images/starwarscover.png" alt="Description 2" style="width: 100%; max-width: 250px;">
    </td>
  </tr>
</table>


**Summary:** You will store lines of a movie script inside a `String[]` array and pass that array through different methods. Because arrays in Java are passed by **reference**, any changes you make to the array inside a method will permanently change the original array in your `main` method.

## Setup

### Part A: Your Codepad
1. Create a [CodePad.app](https://codepad.app/) account with your school Google Account.
1. Go to **MyPads** -> **+New Pad**
1. Select **Java** -> **Create**
1. Name the project `CS2Project1-YOURNAME`.
1. Select the three dots and **Rename** `Main.java` to `ScriptParser.java`
1. Add file and rename it **dialogue.txt**.
1. Add a file and rename it **StringsCheatSheet.txt**.

### Part B: Copy the files
1. Paste the [Starter Code](https://codepad.app/join_pad/qin7rlhl03850480) 
1. Copy and paste the text file **dialogue.txt**.
1. Copy and paste the **StringsCheatSheet.txt**.
1. **Run** the program.  
   
Your setup is correct if you see the following:
![CodePad Setup](../images/cs2_codepad.png)
```java
>  java ScriptParser 
=== 1. RAW INCOMING SCRIPT (First 10 lines) ===

=== 2. CLEANING & STANDARDIZING ===

=== 3. CONTENT MODERATION ===

=== 4. CHARACTER DIALOGUE TRACKING ===
LUKE speaks 0 times in this file.
```

**Follow the comments in the code to complete the project.**
