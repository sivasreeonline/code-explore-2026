# Q003: Consonant Vowel Merge

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Character%20Classification](https://img.shields.io/badge/topic-Character%20Classification-blue) ![topic-Stable%20Ordering](https://img.shields.io/badge/topic-Stable%20Ordering-blue) ![pattern-Stable%20Partitioning%20and%20String%20Construction](https://img.shields.io/badge/pattern-Stable%20Partitioning%20and%20String%20Construction-purple)

## 1. Problem Statement

**Consonant Vowel Merge**

You are given a string S of lowercase English letters, with an equal number of vowels and consonants. You have to rearrange the string such that:

- Consonants are placed at even indices (0, 2, 4, ...)
- Vowels are placed at odd indices (1, 3, 5, ...)
- The relative order of vowels and consonants stays the same as in the original string.

Your task is to rearrange and return the rearranged string.

## 2. Input Format

**Parameters:**

- input1: A string S consisting of lowercase English letters (a-z).

**Standalone program input convention used by the solutions below:**

- Line 1: String S.

## 3. Output Format

Return a string representing the rearranged string.

For the standalone programs, print the rearranged string on one line.

## 4. Constraints

- S contains lowercase English letters.
- S has the same number of vowels and consonants.
- The vowels are a, e, i, o, and u; y is a consonant.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: beauty
~~~

**Output**

~~~text
betayu
~~~

**Explanation**

The vowels, in order, are e, a, u and the consonants in order as b, t, y.

- Place consonants at even indices: b, t, y.
- Place vowels at odd indices: e, a, u.

The resulting string is betayu.

**Standalone input for the programs below**

~~~text
beauty
~~~

### Example 2

**Input**

~~~text
input1: applee
~~~

**Output**

~~~text
papele
~~~

**Explanation**

The vowels, in order, are a, e, e and the consonants in order as p, p, l.

- Place consonants at even indices: p, p, l.
- Place vowels at odd indices: a, e, e.

The resulting string is papele.

**Standalone input for the programs below**

~~~text
applee
~~~

## 6. Intuition

Separate the string into two sequences while preserving the order within each sequence: one for consonants and one for vowels. Then alternate them, placing a consonant at index 0, a vowel at index 1, a consonant at index 2, and so on.

## 7. Brute-Force Approach (When Relevant)

One approach is to collect vowels and consonants in separate lists and then fill the result at the required positions. This is also a clear and efficient solution for the given constraints; repeatedly searching for the next vowel or consonant would add unnecessary work.

## 8. Optimized Approach

**Core idea:** Collect the two character groups in their original relative order, then place each consonant at an even index and its corresponding vowel at the next odd index.

1. Traverse S once and append each character to the vowel or consonant sequence.
2. The input contains equal numbers of vowels and consonants, so the two sequences have equal lengths.
3. For each position in the sequences, place the consonant at index 2i and the vowel at index 2i + 1.
4. Return the result.

## 9. Java Solution

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    public static void main(String[] args) throws Exception
    {
        String s = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (s.isEmpty())
        {
            System.out.println();
            return;
        }

        String vowels = "aeiou";
        StringBuilder vowelCharacters = new StringBuilder();
        StringBuilder consonantCharacters = new StringBuilder();
        for (char ch : s.toCharArray())
        {
            if (vowels.indexOf(ch) >= 0)
            {
                vowelCharacters.append(ch);
            }
            else
            {
                consonantCharacters.append(ch);
            }
        }

        if (vowelCharacters.length() != consonantCharacters.length())
        {
            throw new IllegalArgumentException("S must have an equal number of vowels and consonants.");
        }

        char[] result = new char[s.length()];
        for (int i = 0; i < consonantCharacters.length(); i++)
        {
            result[2 * i] = consonantCharacters.charAt(i);
            result[2 * i + 1] = vowelCharacters.charAt(i);
        }
        System.out.println(new String(result));
    }
}
~~~

## 10. Python Solution

~~~python
import sys

def main():
    s = sys.stdin.read().strip()
    vowels = set("aeiou")
    vowel_characters = [ch for ch in s if ch in vowels]
    consonant_characters = [ch for ch in s if ch not in vowels]

    if len(vowel_characters) != len(consonant_characters):
        raise ValueError("S must have an equal number of vowels and consonants.")

    result = [""] * len(s)
    for i, (consonant, vowel) in enumerate(zip(consonant_characters, vowel_characters)):
        result[2 * i] = consonant
        result[2 * i + 1] = vowel
    print("".join(result))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: S = beauty.

| Original Order | Character Group | Collected Sequence |
| --- | --- | --- |
| e, a, u | Vowels | eau |
| b, t, y | Consonants | bty |

| Output Index | Required Group | Character Placed | Partial Result |
| --- | --- | --- | --- |
| 0 | Consonant | b | b |
| 1 | Vowel | e | be |
| 2 | Consonant | t | bet |
| 3 | Vowel | a | beta |
| 4 | Consonant | y | betay |
| 5 | Vowel | u | betayu |

Final result: betayu.

## 12. Time and Space Complexity

Let L be the length of S.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(L) | The string is traversed to classify characters and the result is filled once. |
| Auxiliary space | O(L) | The vowel sequence, consonant sequence, and result use space proportional to the string length. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| beauty | betayu | Verifies that y is classified as a consonant while character order is preserved |
| applee | papele | Checks stable ordering when the input has repeated vowels and consonants |
| ab | ba | Checks that one consonant and one vowel are placed in their required positions |
| abecid | bacedi | Checks preservation of both character sequences |

## 14. Common Mistakes

- Putting vowels at even indices instead of odd indices.
- Sorting the vowels or consonants, which changes their relative order.
- Treating y as a vowel; in this problem, y is a consonant.
- Assuming the input can have unequal numbers of vowels and consonants.

## 15. Pattern Identification

**Primary pattern:** Stable Partitioning and String Construction

**Why this pattern applies:** Each character belongs to one of two groups, and the task preserves the relative order within each group while placing the groups at alternating indices.

## 16. Key Takeaways

- Preserve the original order within the vowel and consonant groups.
- Place consonants at indices 0, 2, 4, and so on; place vowels at indices 1, 3, 5, and so on.
- Equal group sizes let each vowel-consonant pair fill two adjacent positions.
