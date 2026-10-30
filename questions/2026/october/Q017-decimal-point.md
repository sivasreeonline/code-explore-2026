# Q017: Decimal Point

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-String%20Construction](https://img.shields.io/badge/topic-String%20Construction-blue) ![topic-Enumeration](https://img.shields.io/badge/topic-Enumeration-blue) ![pattern-String%20Construction%20and%20Enumeration](https://img.shields.io/badge/pattern-String%20Construction%20and%20Enumeration-purple)

## 1. Problem Statement

**Decimal Point**

Given an integer S, use its decimal digits in their original order. Insert a decimal point before the first digit, between every pair of adjacent digits, and after the last digit. Prefix the first format with `0` and suffix the last format with `0`. Return all formats in that order, separated by spaces.

## 2. Input Format

**Parameters:**

- input1: An integer value S representing the group of students.

For standalone testing, line 1 contains S.

Platform method signature: `allDecimalPoints(cls, input1)`.

## 3. Output Format

Return a space-separated string of all valid photo formats showing where the camera was placed.

For a string of digits, the formats are listed in this order:

1. Decimal point before the first digit, prefixed with 0.
2. Decimal point between each pair of adjacent digits, from left to right.
3. Decimal point after the last digit, followed by 0.

## 4. Constraints

- S is a non-negative integer.
- Use its decimal digits in their original order.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 123
~~~

**Output**

~~~text
0.123 1.23 12.3 123.0
~~~

**Explanation**

The valid camera positions produce these formats:

- Before the first digit: 0.123
- Between 1 and 2: 1.23
- Between 2 and 3: 12.3
- After the last digit: 123.0

The formats are returned in that order.

### Example 2

**Input**

~~~text
input1: 5
~~~

**Output**

~~~text
0.5 5.0
~~~

**Explanation**

There is one digit, so there is no position between digits. The only valid positions are before the digit and after it, producing 0.5 and 5.0.

## 6. Intuition

A string of length N has N + 1 possible places to insert a decimal point: one before the first digit, N - 1 between digits, and one after the last digit. Construct each format directly from the original digit string.

## 7. Brute-Force Approach (When Relevant)

There is no need for a search over candidate formats. Generate every possible insertion position in order. Each output uses all the original digits exactly once and adds the required prefix or suffix zero where the camera is placed outside the string.

## 8. Optimized Approach

**Core idea:** Generate the leading format, every internal split, and the trailing format.

1. Start the result list with 0. followed by S.
2. For every split position from 1 through the final internal position, append S's prefix, a decimal point, and S's suffix.
3. Append S followed by .0.
4. Join the formats with one space and return the result.

## 9. Java Solution

The Java solution is a standalone program that reads S and prints the formats.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;

public class Main
{
    public static String decimalPoint(String s)
    {
        List<String> formats = new ArrayList<>();
        formats.add("0." + s);

        for (int i = 1; i < s.length(); i++)
        {
            formats.add(s.substring(0, i) + "." + s.substring(i));
        }

        formats.add(s + ".0");
        return String.join(" ", formats);
    }

    public static void main(String[] args) throws Exception
    {
        String s = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (s.isEmpty())
        {
            return;
        }

        System.out.println(decimalPoint(s));
    }
}
~~~

## 10. Python Solution

The Python method is named `allDecimalPoints` and returns a string.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def allDecimalPoints(cls, input1):
        '''
        input1 : int (converted to a digit string internally)
        Expected return type : string
        '''
        # Read only region end
        # Write code here
        s = str(input1)
        formats = ["0." + s]

        for i in range(1, len(s)):
            formats.append(s[:i] + "." + s[i:])

        formats.append(s + ".0")
        return " ".join(formats)
~~~

## 11. Dry Run

Example 1: S = 123.

| Position | Format | Reason |
| --- | --- | --- |
| Before the first digit | 0.123 | Prefix with 0. |
| Between 1 and 2 | 1.23 | Split after the first digit |
| Between 2 and 3 | 12.3 | Split after the second digit |
| After the last digit | 123.0 | Append .0 |

Final result: 0.123 1.23 12.3 123.0.

## 12. Time and Space Complexity

Let N be the number of digits in S.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N^2) in total output characters | There are N + 1 formats and the total length of all copied substrings grows quadratically. |
| Auxiliary space | O(N^2) for the returned formats | The output itself contains N + 1 strings of length approximately N. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| 123 | 0.123 1.23 12.3 123.0 | Generates every possible decimal-point position for a three-digit input |
| 5 | 0.5 5.0 | Checks all decimal positions for a single-digit input |
| 12 | 0.12 1.2 12.0 | One internal position |
| 9 | 0.9 9.0 | Single digit |
| 60004 | 0.60004 6.0004 60.004 600.04 6000.4 60004.0 | Includes zeros within the number |

## 14. Common Mistakes

- Omitting the format before the first digit or after the last digit.
- Adding a decimal point between characters in the wrong order.
- Changing or reordering the original digits.
- Returning a list when the required output is one space-separated string.
- Treating the narrative about students as a requirement to add or remove digits.

## 15. Pattern Identification

**Primary pattern:** String Construction and Enumeration

**Why this pattern applies:** Every possible decimal-point position is enumerated in a fixed left-to-right order, and each output is formed from a prefix and suffix of the original digit string.

## 16. Key Takeaways

- A string with N digits has N + 1 decimal-point positions.
- The first format begins with 0., and the last format ends with .0.
- Preserve the order of all digits and join formats with a single space.
- The Python method uses the required signature `allDecimalPoints(cls, input1)`.
