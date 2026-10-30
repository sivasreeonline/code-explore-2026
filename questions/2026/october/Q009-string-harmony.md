# Q009: String Harmony

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Contiguous%20Substrings](https://img.shields.io/badge/topic-Contiguous%20Substrings-blue) ![topic-Character%20Traversal](https://img.shields.io/badge/topic-Character%20Traversal-blue) ![pattern-String%20Matching%20and%20Incremental%20Pattern%20Validation](https://img.shields.io/badge/pattern-String%20Matching%20and%20Incremental%20Pattern%20Validation-purple)

## 1. Problem Statement

**String Harmony**

A harmonic pattern in a string is a pattern where the string starts with 'ab' and repeats itself with one additional alphabet in alphabetical order. The moment this pattern breaks, the string is no longer in harmonic pattern.

You are given a string S, and you have to find and return an integer value representing the largest contiguous substring which is in harmonic pattern.

A harmonic substring is formed by concatenating the blocks "ab", "abc", "abcd", and so on, with each block containing one more alphabet character than the previous block.

## 2. Input Format

**Parameters:**

- input1: A string value S.

Platform method signature: `stringHarmony(cls, input1)`, returning an integer. For standalone Java testing, line 1 contains S.

## 3. Output Format

Return an integer representing the length of the longest contiguous substring that follows the harmonic pattern.

For the standalone programs, print the integer on one line.

## 4. Constraints

- S contains lowercase English letters from a to z.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: ababcabc
~~~

**Output**

~~~text
5
~~~

**Explanation**

The substring "ababc" consists of the block "ab" followed by the block "abc". The next required block would be "abcd", but the remaining characters do not complete that block. The longest harmonic substring in this input has length 5.

### Example 2

**Input**

~~~text
ababcabcdabcde
~~~

**Output**

~~~text
14
~~~

**Explanation**

The complete blocks are `ab`, `abc`, `abcd`, and `abcde`. Their total length is 2 + 3 + 4 + 5 = 14.

## 6. Intuition

A harmonic substring consists of consecutive blocks: first "ab", then "abc", then "abcd", and so on. Each new block starts at the next position and contains one more character than the previous block. Try each position where a candidate begins with "ab" and extend it only while each entire next block matches the expected alphabetical prefix.

## 7. Brute-Force Approach (When Relevant)

A brute-force approach could examine every substring beginning with 'a' and test whether its characters continue alphabetically. That repeats work across overlapping substrings. The optimized solution tracks the current valid run in one traversal.

## 8. Optimized Approach

**Core idea:** Match each candidate substring against the required blocks of increasing length.

1. Scan the string for each position whose next two characters are "ab".
2. Starting at that position, match the complete blocks "ab", "abc", "abcd", and so on.
3. Stop extending that candidate at the first block that does not match completely.
4. Record the total number of matched characters and keep the maximum across all starting positions.
5. Return the maximum length.

Since the block length can increase only from 2 through 26 for lowercase English letters, at most 25 blocks need to be checked for each starting position.

## 9. Java Solution

The Java solution is a standalone program that reads S and prints the result.

~~~java
import java.util.Scanner;

public class Main
{
    private static int longestHarmonicSubstring(String s)
    {
        String alphabet = "abcdefghijklmnopqrstuvwxyz";
        int maxLength = 0;

        for (int start = 0; start + 1 < s.length(); start++)
        {
            if (s.charAt(start) != 'a' || s.charAt(start + 1) != 'b')
            {
                continue;
            }

            int position = start;
            int blockLength = 2;

            while (blockLength <= alphabet.length()
                && position + blockLength <= s.length())
            {
                String expected = alphabet.substring(0, blockLength);
                if (!s.regionMatches(position, expected, 0, blockLength))
                {
                    break;
                }

                position += blockLength;
                maxLength = Math.max(maxLength, position - start);
                blockLength++;
            }
        }

        return maxLength;
    }

    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        System.out.println(longestHarmonicSubstring(s));
        sc.close();
    }
}
~~~

## 10. Python Solution

The Python method preserves the platform signature and return type.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def stringHarmony(cls, input1):
        '''
        input1 : string
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        s = input1
        alphabet = "abcdefghijklmnopqrstuvwxyz"
        max_length = 0

        for start in range(len(s) - 1):
            if s[start:start + 2] != "ab":
                continue

            position = start
            block_length = 2

            while (
                block_length <= len(alphabet)
                and position + block_length <= len(s)
            ):
                expected = alphabet[:block_length]
                if s[position:position + block_length] != expected:
                    break

                position += block_length
                max_length = max(max_length, position - start)
                block_length += 1

        return max_length
~~~

## 11. Dry Run

Example 1: S = ababcabc.

| Candidate Block | Expected Block | Actual Block | Result |
| --- | --- | --- | --- |
| 1 | ab | ab | Match; current length 2 |
| 2 | abc | abc | Match; current length 5 |
| 3 | abcd | abc (only three characters remain) | Does not match completely; stop |

The longest complete harmonic sequence has length 5.

## 12. Time and Space Complexity

Let L be the length of S.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(L) for a fixed 26-letter alphabet | Each starting position checks at most 25 blocks, a constant bound. |
| Auxiliary space | O(1) | The alphabet is fixed and only indices and counters are stored. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| ababcabc | 5 | Counts the total length of the longest consecutive chain of alphabet-prefix blocks |
| ababcabcdabcde | 14 | Complete blocks ab + abc + abcd + abcde |
| abc | 2 | Only the first block is complete |
| ababcd | 2 | The next expected block is abc, but the remaining text does not match |
| xyz | 0 | No candidate starts with ab |
| ababc | 5 | Two complete blocks |

## 14. Common Mistakes

- Counting alphabetical characters anywhere in the string rather than a contiguous substring.
- Starting a valid run at a character other than 'a'.
- Forgetting to update the maximum after the current run changes.
- Continuing a run after the expected next letter is missing.
- Changing the required pattern or skipping a valid complete block.

## 15. Pattern Identification

**Primary pattern:** String Matching and Incremental Pattern Validation

**Why this pattern applies:** Every candidate begins with "ab", and the expected blocks grow by one character each time. The candidate is extended only when the next complete block matches.

## 16. Key Takeaways

- A harmonic sequence consists of the consecutive blocks "ab", "abc", "abcd", and so on.
- Only complete blocks extend a valid candidate.
- The longest candidate is the maximum length among all valid starting positions.
