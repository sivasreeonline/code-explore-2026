# Q016: Count Surrounded Digit

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Digit%20Runs](https://img.shields.io/badge/topic-Digit%20Runs-blue) ![topic-Character%20Classification](https://img.shields.io/badge/topic-Character%20Classification-blue) ![pattern-Single-Pass%20String%20Traversal](https://img.shields.io/badge/pattern-Single-Pass%20String%20Traversal-purple)

## 1. Problem Statement

**Count Surrounded Digit**

You are given a string S containing lowercase letters (a-z) and digits (0-9). You have to count how many digit substrings are surrounded by letters on both sides.

A digit substring is a sequence of one or more consecutive digits, and it is only counted if there is a lowercase letter immediately before and after it. Your task is to find and return an integer value representing the total count of such substrings.

## 2. Input Format

**Parameters:**

- input1: A single string S consisting of lowercase English letters (a-z) and digits (0-9).

For standalone testing, line 1 contains S.

The standalone solutions use the helper name `countSurroundedDigit`.

## 3. Output Format

Return an integer value representing the count of digit substrings that are surrounded on both sides by lowercase letters.

For standalone programs, print the count on one line.

## 4. Constraints

- S contains lowercase English letters and digits.
- A digit substring is a maximal run of one or more consecutive digits.
- Count a run only when a lowercase letter appears immediately before it and immediately after it.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: a123d
~~~

**Output**

~~~text
1
~~~

**Explanation**

The digit substring 123 is immediately preceded by a and immediately followed by d. Therefore, one digit substring is counted.

### Example 2

**Input**

~~~text
input1: 123a45b67c
~~~

**Output**

~~~text
2
~~~

**Explanation**

The digit substrings are:

- 123 is at the beginning of the string, so it has no lowercase letter immediately before it and is not counted.
- 45 is surrounded by a and b, so it is counted.
- 67 is surrounded by b and c, so it is counted.

The total count is 2.

## 6. Intuition

Process the string from left to right and identify each maximal run of consecutive digits. A run counts only when the character immediately before the run and the character immediately after it are both lowercase English letters.

## 7. Brute-Force Approach (When Relevant)

For each position containing a digit, scan left and right to determine its digit run and check the surrounding characters. This can revisit the same run multiple times. A single traversal that consumes each digit run once is simpler and more efficient.

## 8. Optimized Approach

**Core idea:** Scan each consecutive digit run exactly once and test the characters at its boundaries.

1. Start at the first character.
2. If it is a letter, advance to the next character.
3. If it is a digit, remember the run's start and advance until the run ends.
4. Check whether the preceding and following characters both exist and are lowercase letters.
5. If both conditions hold, increment the answer.
6. Return the answer.

## 9. Java Solution

The Java solution is a standalone program that reads S and prints the count.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    public static int countSurroundedDigit(String s)
    {
        int count = 0;
        int i = 0;

        while (i < s.length())
        {
            if (!Character.isDigit(s.charAt(i)))
            {
                i++;
                continue;
            }

            int start = i;
            while (i < s.length() && Character.isDigit(s.charAt(i)))
            {
                i++;
            }

            boolean hasLowercaseLeft = start > 0
                && s.charAt(start - 1) >= 'a'
                && s.charAt(start - 1) <= 'z';
            boolean hasLowercaseRight = i < s.length()
                && s.charAt(i) >= 'a'
                && s.charAt(i) <= 'z';

            if (hasLowercaseLeft && hasLowercaseRight)
            {
                count++;
            }
        }

        return count;
    }

    public static void main(String[] args) throws Exception
    {
        String s = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        System.out.println(countSurroundedDigit(s));
    }
}
~~~

## 10. Python Solution

The Python method uses the helper name `countSurroundedDigit` and returns an integer.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def countSurroundedDigit(cls, input1):
        '''
        input1 : string
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        s = input1
        count = 0
        i = 0

        while i < len(s):
            if not s[i].isdigit():
                i += 1
                continue

            start = i
            while i < len(s) and s[i].isdigit():
                i += 1

            has_lowercase_left = (
                start > 0 and 'a' <= s[start - 1] <= 'z'
            )
            has_lowercase_right = (
                i < len(s) and 'a' <= s[i] <= 'z'
            )

            if has_lowercase_left and has_lowercase_right:
                count += 1

        return count
~~~

## 11. Dry Run

Example 2: S = 123a45b67c.

| Digit Run | Character Before | Character After | Counted? | Running Count |
| --- | --- | --- | --- | --- |
| 123 | Start of string | a | No | 0 |
| 45 | a | b | Yes | 1 |
| 67 | b | c | Yes | 2 |

Final answer: 2.

## 12. Time and Space Complexity

Let L be the length of S.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(L) | Each character is visited once. |
| Auxiliary space | O(1) | Only indices, boundary checks, and a counter are stored. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| a123d | 1 | Counts a single digit run bounded by lowercase letters on both sides |
| 123a45b67c | 2 | Counts only digit runs with lowercase letters immediately before and after them |
| 123 | 0 | Digit run has no surrounding letters |
| a123 | 0 | Missing a lowercase letter after the run |
| 123b | 0 | Missing a lowercase letter before the run |
| a12b34c | 2 | Two separate digit runs, both surrounded |
| a1b2c | 2 | Two one-digit runs, each bounded by lowercase letters |

## 14. Common Mistakes

- Counting every digit instead of counting each consecutive digit run once.
- Counting a run that touches the beginning or end of the string.
- Checking only one side of a digit run.
- Counting digits separated by a letter as a single substring.
- Treating uppercase letters as valid boundaries; only lowercase letters count.

## 15. Pattern Identification

**Primary pattern:** Single-Pass String Traversal

**Why this pattern applies:** Each digit substring is a maximal consecutive run. Its validity depends only on the two characters at its boundaries, so every run can be checked once.

## 16. Key Takeaways

- A digit substring consists of one or more consecutive digits.
- It is counted only when a lowercase letter immediately precedes it and another lowercase letter immediately follows it.
- Count digit runs, not individual digits.
