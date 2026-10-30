# Q018: Minimum Number of Key Presses

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Dynamic%20Programming](https://img.shields.io/badge/topic-Dynamic%20Programming-blue) ![topic-Greedy%20Counting](https://img.shields.io/badge/topic-Greedy%20Counting-blue) ![pattern-Dynamic%20Programming%20on%20String%20Prefixes](https://img.shields.io/badge/pattern-Dynamic%20Programming%20on%20String%20Prefixes-purple)

## 1. Problem Statement

**Minimum Number of Key Presses**

George has a setup which includes a special keyboard and a monitor that initially displays 0. The special keyboard has 11 numeric keys [0,1,2,3,4,5,6,7,8,9,00]. If he presses 00, the previously displayed value will be multiplied by 100. Whereas, if he presses any other numeric key, the previously displayed value will first be multiplied by 10, and then the number on the key will be added to it.

You are given a numeric string S. Your task is to help George find and return an integer value representing the minimum number of key presses to reach the number.

## 2. Input Format

**Parameters:**

- input1: A numeric string S, representing the final number.

For standalone testing, line 1 contains S.

The standalone solutions use the helper name `minimumKeyPresses`.

## 3. Output Format

Return an integer value representing the minimum number of key presses to reach the number.

For standalone programs, print the minimum count on one line.

## 4. Constraints

- S is the decimal representation of a non-negative integer, with no leading zeros except for "0".
- The keyboard has digit keys 0 through 9 and a key labeled 00.
- The 00 key can enter two consecutive zeroes after the number has started.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 60004
~~~

**Output**

~~~text
4
~~~

**Explanation**

Initially, the monitor displays 0.

- Press 6, and the monitor displays 6.
- Press 00, and the monitor displays 600.
- Press 0, and the monitor displays 6000.
- Press 4, and the monitor displays 60004.

Since 4 is the minimum number of key presses, 4 is returned.

### Example 2

**Input**

~~~text
input1: 1355506027
~~~

**Output**

~~~text
10
~~~

**Explanation**

The numeric string is 1355506027. There are no adjacent zeroes that can be entered together using the 00 key, so each digit is pressed separately. There are 10 digits, giving a total of 10 key presses.

## 6. Intuition

Every digit normally costs one key press. The special 00 key can enter two consecutive zeroes with one press instead of two. Therefore, the only way to reduce the number of presses is to replace a pair of adjacent zero digits with one 00 press.

## 7. Brute-Force Approach (When Relevant)

A recursive approach could consider pressing a single digit or, when the next two digits are 00, pressing the special 00 key. Dynamic programming avoids recomputing the same prefixes and finds the minimum in one traversal.

## 8. Optimized Approach

**Core idea:** Use dynamic programming over prefixes of the numeric string.

1. Let dp[i] be the minimum number of key presses required to enter the first i digits.
2. The default choice is to enter the i-th digit individually, giving dp[i - 1] + 1.
3. If the last two digits of that prefix are 00, the special key can enter them in one press, giving dp[i - 2] + 1.
4. Take the smaller of those choices.
5. Return dp[N], where N is the number of digits.

This works because the only multi-digit key is 00, so every possible saving comes from replacing two adjacent zero digits with one key press.

## 9. Java Solution

The Java solution is a standalone program that reads S and prints the minimum count.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    public static int minimumKeyPresses(String s)
    {
        int n = s.length();
        int[] dp = new int[n + 1];

        for (int i = 1; i <= n; i++)
        {
            dp[i] = dp[i - 1] + 1;

            if (i >= 2 && s.charAt(i - 2) == '0' && s.charAt(i - 1) == '0')
            {
                dp[i] = Math.min(dp[i], dp[i - 2] + 1);
            }
        }

        return dp[n];
    }

    public static void main(String[] args) throws Exception
    {
        String s = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (s.isEmpty())
        {
            return;
        }

        System.out.println(minimumKeyPresses(s));
    }
}
~~~

## 10. Python Solution

The Python method uses the helper name `minimumKeyPresses` and returns an integer.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def minimumKeyPresses(cls, input1):
        '''
        input1 : string
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        s = str(input1)
        n = len(s)
        dp = [0] * (n + 1)

        for i in range(1, n + 1):
            dp[i] = dp[i - 1] + 1

            if i >= 2 and s[i - 2:i] == "00":
                dp[i] = min(dp[i], dp[i - 2] + 1)

        return dp[n]
~~~

## 11. Dry Run

Example 1: S = 60004.

| Prefix | Best Last Key Choice | Minimum Presses |
| --- | --- | --- |
| 6 | Press 6 | 1 |
| 60 | Press 0 after 6 | 2 |
| 600 | Press 00 after 6 | 2 |
| 6000 | Press another 0 after 600 | 3 |
| 60004 | Press 4 after 6000 | 4 |

The minimum number of key presses is 4.

## 12. Time and Space Complexity

Let N be the number of digits in S.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N) | Each prefix is processed once. |
| Auxiliary space | O(N) | The dynamic-programming array stores one value per prefix. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| 60004 | 4 | Counts key presses using the special 00 key for runs of zeroes |
| 1355506027 | 10 | Counts presses for a longer number containing multiple zero runs |
| 5 | 1 | One digit |
| 100 | 2 | Press 1 followed by 00 |
| 10000 | 3 | Press 1 followed by 00 twice |
| 100000 | 4 | Five trailing zeroes require two 00 presses and one 0 |
| 123456 | 6 | No adjacent zeroes; each digit is entered separately |
| 6000004 | 5 | 6, 00, 00, 0, 4 |

## 14. Common Mistakes

- Treating the special 00 key as if it entered a single zero.
- Counting every zero pair independently without ensuring the pairs do not overlap.
- Forgetting that digit keys still require one press each.
- Returning the number of digits instead of accounting for the 00 key.
- Treating the starting display value 0 as a digit already entered by the user.

## 15. Pattern Identification

**Primary pattern:** Dynamic Programming on String Prefixes

**Why this pattern applies:** For each prefix, the last key can either be a single digit or the special 00 key if the prefix ends in two zeroes. The optimum depends on the best solutions for one or two shorter prefixes.

## 16. Key Takeaways

- Every ordinary digit costs one key press.
- Two adjacent zeroes can be entered with one press using the special 00 key.
- Dynamic programming checks each prefix once and avoids overlapping-pair mistakes.
