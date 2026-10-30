# Q028: Alphabet Block Harmony

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Pattern%20Matching](https://img.shields.io/badge/topic-Pattern%20Matching-blue) ![topic-Enumeration](https://img.shields.io/badge/topic-Enumeration-blue) ![pattern-String%20Matching%20with%20Enumeration](https://img.shields.io/badge/pattern-String%20Matching%20with%20Enumeration-purple)

## 1. Problem Statement

Starting at any index, match consecutive blocks 'ab', then 'abc', then 'abcd', and so on. Return the largest total length of a chain.

## 2. Input Format

- s: A string.

For standalone programs, line 1 contains the input string.

## 3. Output Format

Return an integer representing the largest total length of a consecutive chain of matching alphabet blocks.

## 4. Constraints

- The input string contains lowercase English letters.

## 5. Examples and Explanations

### Example 1

~~~text
Input: xxababcabcdz
Output: 9
~~~

**Explanation**

Starting at the first 'a' in this string, the blocks 'ab', 'abc', and 'abcd' match consecutively. Their combined length is 2 + 3 + 4 = 9. The next block would need five characters, 'abcde', but the remaining input does not match it.

## 6. Intuition

Try every position as a possible start. At each start, the first block must be 'ab'. Each next block begins immediately after the previous one and must equal the alphabet prefix with one additional letter. Stop a candidate at the first missing or incorrect block.

## 7. Brute-Force Approach (When Relevant)

For each starting position, compare each required block character by character. Track the total matched length and retain the maximum over all starting positions.

## 8. Optimized Approach

**Core idea:** Test only sequential alphabet prefixes and stop as soon as one block does not match.

1. Iterate through every start position.
2. Set the next block length to 2.
3. Check whether the substring at the current position equals the first block-length letters of the alphabet.
4. If it matches, add its length to the chain and advance the current position by that block length.
5. Increase the required block length and repeat.
6. Return the maximum total length over all start positions.

The Java solution checks each possible starting position. Because the alphabet has 26 letters, a chain has at most 25 blocks.

## 9. Java Solution

The method `stringHarmony` takes a string parameter. The `UserMainCode` wrapper matches that interface; the main method is a local test driver.

~~~java
import java.util.Scanner;

class UserMainCode
{
    public int stringHarmony(String s)
    {
        if (s == null || s.length() < 2)
        {
            return 0;
        }

        int n = s.length();
        int max = 0;

        for (int start = 0; start < n; start++)
        {
            int index = start;
            int total = 0;
            int k = 1;

            while (k < 26)
            {
                int len = k + 1;
                if (index + len > n)
                {
                    break;
                }

                boolean match = true;
                for (int j = 0; j <= k; j++)
                {
                    if (s.charAt(index + j) != (char) ('a' + j))
                    {
                        match = false;
                        break;
                    }
                }

                if (!match)
                {
                    break;
                }

                total += len;
                index += len;
                k++;
            }

            max = Math.max(max, total);
        }

        return max;
    }
}

public class Main
{
    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        System.out.println(new UserMainCode().stringHarmony(s));
        sc.close();
    }
}
~~~

## 10. Python Solution

~~~python
def stringHarmony(s):
    if s is None or len(s) < 2:
        return 0

    n = len(s)
    best = 0
    alphabet = "abcdefghijklmnopqrstuvwxyz"

    for start in range(n):
        position = start
        total = 0
        block_length = 2

        while block_length <= len(alphabet):
            end = position + block_length
            if end > n:
                break

            if s[position:end] != alphabet[:block_length]:
                break

            total += block_length
            position = end
            best = max(best, total)
            block_length += 1

    return best

if __name__ == "__main__":
    print(stringHarmony(input().strip()))
~~~

## 11. Dry Run

For s = xxababcabcdz:

| Block | Expected substring | Actual substring | Running total |
| --- | --- | --- | --- |
| 1 | ab | ab | 2 |
| 2 | abc | abc | 5 |
| 3 | abcd | abcd | 9 |
| 4 | abcde | Remaining text does not match | Stop |

Result: 9.

## 12. Time and Space Complexity

Let n be the length of s.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(n) for a fixed alphabet | Each start checks a bounded number of blocks and at most 26 characters per block. |
| Auxiliary space | O(1) | Only counters and the fixed alphabet are used. |

## 13. Edge Cases and Test Cases

| Input | Expected output | Purpose |
| --- | --- | --- |
| xxababcabcdz | 9 | Finds the consecutive blocks ab, abc, and abcd and totals their lengths |
| ababcabcd | 9 | Three complete blocks |
| ababc | 5 | Two complete blocks |
| xyz | 0 | No complete block begins with ab |
| ab | 2 | Smallest valid block |
| abcde | 2 | The string does not contain the next block immediately after ab |

## 14. Common Mistakes

- Requiring the chain to begin at index zero.
- Accepting blocks out of order or with gaps between them.
- Increasing the block length without checking the complete next block.
- Returning the number of blocks rather than the sum of their lengths.
- Applying Q009's rule instead of the block pattern defined here.

## 15. Pattern Identification

**Primary pattern:** String Matching with Enumeration

**Why this pattern applies:** Each possible start is checked, then extended by exact alphabet-prefix blocks of increasing length.

## 16. Key Takeaways

- A valid chain contains consecutive blocks ab, abc, abcd, and so on.
- The chain ends at the first incomplete or mismatching block.
- Keep the maximum total matched character count.
