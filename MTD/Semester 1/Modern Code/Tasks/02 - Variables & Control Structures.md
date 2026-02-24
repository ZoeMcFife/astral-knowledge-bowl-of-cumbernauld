#modern_code #java 

## Exercise 1 (4 points) — Rectangle Calculator

**File:** `RectangleCalculator.java`

### Requirements
- Declare variables for **width** and **height** (both `int`).
- Calculate and display: **area**, **perimeter**, and **diagonal length**.
- Use proper variable names and include comments.
- Test with different values for width and height.
- Display results with proper formatting (round diagonal to 2 decimal places).

### Notes
- Use `Math.sqrt()` to compute the diagonal.

### Expected Output
    === Rectangle Calculator ===
    Width: 5
    Height: 3
    Area: 15
    Perimeter: 16
    Diagonal length: 5.83

---

## Exercise 2 (4 points) — FizzBuzz Game

**File:** `FizzBuzz.java`

### Requirements
- Use a variable to store the maximum number to count to.
- Check divisibility by **3** and **5**.
- Display:
  - `"Fizz"` for numbers divisible by 3
  - `"Buzz"` for numbers divisible by 5
  - `"FizzBuzz"` for numbers divisible by both
  - The number itself otherwise
- Use proper variable names and include comments.

### Expected Output
    === FizzBuzz Game ===
    Counting from 1 to 15:
    1
    2
    Fizz
    4
    Buzz
    Fizz
    7
    8
    Fizz
    Buzz
    11
    Fizz
    13
    14
    FizzBuzz

---

## Exercise 3 (4 points) — Day of Week Processor

**File:** `DayOfWeek.java`

### Requirements
- Use a variable to store a **day number (1–7)**.
- Display the **day name** and **type**.
- Display whether it’s a **Weekday** or **Weekend**.
- Handle invalid input appropriately.
- Test with different day numbers.
- Use proper variable names and include comments.

### Expected Output
    === Day of Week Processor ===
    Day number: 3
    Day: Wednesday
    Type: Weekday

![[Pasted image 20251027144349.png]]

---

## Exercise 4 (4 points) — Star Pattern Generator

**File:** `StarPattern.java`

### Requirements
- Use a variable to control the **pattern size** (number of rows).
- Display a **right triangle pattern of stars**.
- Use proper variable names and include comments.
- Test with different pattern sizes.

### Expected Output
    === Star Pattern Generator ===
    Pattern size: 5
    *
    * *
    * * *
    * * * *
    * * * * *

---

## Exercise 5 (4 points) — Collatz Conjecture

**File:** `CollatzConjecture.java`

### Requirements
- Start with a given number.
- If the number is **even**, divide it by 2.
- If the number is **odd**, multiply by 3 and add 1.
- Repeat until you reach 1.
- Display each step and count the total steps.
- Use proper variable names and include comments.

### Expected Output
    === Collatz Conjecture ===
    Starting with 5:
    5 (odd) -> 16
    16 (even) -> 8
    8 (even) -> 4
    4 (even) -> 2
    2 (even) -> 1
    Reached 1 in 5 steps!

---

## Exercise 6 (4 points) — Dice Rolling Simulator

**File:** `DiceRollingSimulator.java`

### Requirements
- Use a variable for the **target number (1–6)**.
- Roll dice until you get the target number.
- Count how many rolls it takes.
- Display each roll and the final count.
- Use proper variable names and include comments.

### Random Number Generation
Use `Math.random()` which returns a decimal between 0.0 and 1.0.  
To get integers `1..6`:

    (int)(Math.random() * 6) + 1

### Expected Output
    === Dice Rolling Simulator ===
    Target number: 6
    Roll 1: 3
    Roll 2: 1
    Roll 3: 4
    Roll 4: 6
    Found target number 6 in 4 rolls!

---

> “**Important**: *For exercises 2 and 3, think about whether if/else or switch would be the better*
*choice for the problem. For exercises 4-6, consider which loop type (for, while, or do-while)*
*is most appropriate for the task*”

![[A02_Variables_Control.pdf]]

![[MC_UE02_BUNEA 1.pdf]]