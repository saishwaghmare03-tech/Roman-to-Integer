# 🏛️ Roman to Integer (LeetCode #13)

[![Java](https://img.shields.io/badge/Language-Java-orange.svg)](https://www.java.com)
[![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen.svg)](https://leetcode.com/problems/roman-to-integer/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A lightning-fast and memory-efficient Java solution to convert Roman numerals to integers using an optimized single-pass `switch` approach.

---

## 📖 Problem Overview

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D`, and `M`.

| Symbol | Value |
| :---: | :---: |
| **I** | 1 |
| **V** | 5 |
| **X** | 10 |
| **L** | 50 |
| **C** | 100 |
| **D** | 500 |
| **M** | 1000 |

Normally, Roman numerals are written from largest to smallest left to right. However, six subtraction cases exist where a smaller value precedes a larger one:
* **`I`** before **`V`** (5) or **`X`** (10) makes `4` and `9`.
* **`X`** before **`L`** (50) or **`C`** (100) makes `40` and `90`.
* **`C`** before **`D`** (500) or **`M`** (1000) makes `400` and `900`.

---

## 💡 Solution Architecture

Instead of overhead-heavy hash maps, this implementation handles conversion in a single left-to-right pass using a native Java `switch` statement:
1. **Accumulate:** Adds the standard value of the current symbol to the running total (`val`).
2. **Track Context:** Keeps track of the `priorSymbol`.
3. **Handle Subtraction:** If a valid subtraction pairing is found (e.g., `'I'` followed by `'V'`), it subtracts twice the value of the previous symbol to correct the addition made in the prior iteration.
4. **Iterate:** Updates `priorSymbol` and moves to the next character.

---

## ⚡ Complexity Analysis

* **Time Complexity:** $\mathcal{O}(N)$ — Traverses the input string of length $N$ exactly once.
* **Space Complexity:** $\mathcal{O}(1)$ — Uses a fixed amount of auxiliary memory regardless of input size.

---

## 💻 Implementation (`Solution.java`)

```java
public class Solution {
    public int romanToInt(String s) {
        char priorSymbol = 0;
        int val = 0;
        
        for (int i = 0; i < s.length(); i++) {
            switch (s.charAt(i)) {
                case 'I':
                    val += 1;
                    priorSymbol = 'I';
                    break;
                case 'V':
                    val += 5;
                    if (priorSymbol == 'I') {
                        val -= 2;
                    }
                    priorSymbol = 'V';
                    break;
                case 'X':
                    val += 10;
                    if (priorSymbol == 'I') {
                        val -= 2;
                    }
                    priorSymbol = 'X';
                    break;
                case 'L':
                    val += 50;
                    if (priorSymbol == 'X') {
                        val -= 20;
                    }
                    priorSymbol = 'L';
                    break;
                case 'C':
                    val += 100;
                    if (priorSymbol == 'X') {
                        val -= 20;
                    }
                    priorSymbol = 'C';
                    break;
                case 'D':
                    val += 500;
                    if (priorSymbol == 'C') {
                        val -= 200;
                    }
                    priorSymbol = 'D';
                    break;
                case 'M':
                    val += 1000;
                    if (priorSymbol == 'C') {
                        val -= 200;
                    }
                    priorSymbol = 'M';
                    break;
            }
        }
        return val;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();

        // Test Cases
        System.out.println(solution.romanToInt("III"));      // Output: 3
        System.out.println(solution.romanToInt("LVIII"));    // Output: 58
        System.out.println(solution.romanToInt("MCMXCIV"));  // Output: 1994
    }
}
