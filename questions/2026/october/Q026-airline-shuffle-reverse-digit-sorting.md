# Q026: The Airline Shuffle — Reverse-Digit Sorting

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Sorting](https://img.shields.io/badge/topic-Sorting-blue) ![topic-Custom%20Comparator](https://img.shields.io/badge/topic-Custom%20Comparator-blue) ![pattern-Custom%20Comparator%20Sorting](https://img.shields.io/badge/pattern-Custom%20Comparator%20Sorting-purple)

## 1. Problem Statement

Sort numbers by the lexicographical order of their reversed digit strings. If reversed strings tie, sort the original numbers ascending.

## 2. Input Format

- n: Number of values.
- arr: Array of n integers.

For the standalone programs, line 1 contains n; line 2 contains the array values.

## 3. Output Format

Return the numbers sorted by the lexicographical order of their reversed digit strings. When those reversed strings are equal, order the original numbers numerically in ascending order.

## 4. Constraints

- N is non-negative.
- The array contains N non-negative flight numbers.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 4
input2: {221, 321, 456, 1111}
~~~

**Output**

~~~text
{1111, 221, 321, 456}
~~~

**Explanation**

The reversed digit strings are "122", "123", "654", and "1111". Sorting those strings lexicographically gives "1111", "122", "123", "654", so the corresponding original values are returned in this order.

### Example 2

**Input**

~~~text
input1: 5
input2: {100, 2001, 1110, 321, 10101}
~~~

**Output**

~~~text
{100, 1110, 2001, 10101, 321}
~~~

**Explanation**

The reversed digit strings are "001", "1002", "0111", "123", and "10101". Sorting those strings lexicographically gives "001", "0111", "1002", "10101", "123", so the corresponding original values are returned in this order.

## 6. Intuition

Reverse the decimal representation of every number and compare the reversed values as strings, not as integers. Keep each original number paired with its reversed string so the original number can be returned after sorting.

## 7. Brute-Force Approach (When Relevant)

Use a simple comparison sort. For each pair of values, form their reversed digit strings and compare them lexicographically. If the reversed strings are identical, compare the original numbers in ascending order.

## 8. Optimized Approach

**Core idea:** Precompute each reversed string and use it as the primary sorting key.

1. Convert each number to its decimal string.
2. Reverse that string.
3. Sort records by reversed string, then by original numeric value.
4. Return the original numbers in sorted order.

Precomputing the reversed strings avoids rebuilding them on every comparison.

## 9. Java Solution

Platform method signature: `public int[] theShuffle(int input1, int[] input2)`. The standalone driver reads N followed by N flight numbers.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Arrays;

// Read only region start
class UserMainCode
{
    public int[] theShuffle(int input1, int[] input2)
    {
        // Read only region end
        // Write code here...
        if (input2 == null || input1 <= 0)
        {
            return new int[0];
        }

        int n = Math.min(input1, input2.length);
        Integer[] indices = new Integer[n];
        String[] reversed = new String[n];

        for (int i = 0; i < n; i++)
        {
            indices[i] = i;
            reversed[i] = new StringBuilder(Integer.toString(input2[i])).reverse().toString();
        }

        Arrays.sort(indices, (i, j) ->
        {
            int comparison = reversed[i].compareTo(reversed[j]);
            if (comparison != 0)
            {
                return comparison;
            }
            return Integer.compare(input2[i], input2[j]);
        });

        int[] result = new int[n];
        for (int i = 0; i < n; i++)
        {
            result[i] = input2[indices[i]];
        }

        return result;
    }
}

public class Main
{
    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] tokens = raw.split("\\s+");
        int n = Integer.parseInt(tokens[0]);
        int[] numbers = new int[n];
        for (int i = 0; i < n; i++)
        {
            numbers[i] = Integer.parseInt(tokens[i + 1]);
        }

        System.out.println(Arrays.toString(new UserMainCode().theShuffle(n, numbers)));
    }
}
~~~

## 10. Python Solution

The Python version uses the same method name and parameter order.

~~~python
def theShuffle(input1, input2):
    if input2 is None or input1 <= 0:
        return []

    values = input2[:input1]
    return sorted(values, key=lambda value: (str(value)[::-1], value))

def main():
    import sys
    tokens = list(map(int, sys.stdin.read().split()))
    if not tokens:
        return
    n = tokens[0]
    print(theShuffle(n, tokens[1:n + 1]))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1 reverses the input values to strings and sorts those strings lexicographically:

| Original number | Reversed string | Sorted position |
| --- | --- | --- |
| 221 | 122 | 2 |
| 321 | 123 | 3 |
| 456 | 654 | 4 |
| 1111 | 1111 | 1 |

The resulting order of original numbers is [1111, 221, 321, 456].

## 12. Time and Space Complexity

Let n be the number of values and d the maximum number of digits in a value.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(n log n × d) | Sorting performs O(n log n) comparisons; string comparisons can examine up to d characters. |
| Auxiliary space | O(n × d) | Reversed strings and sorting records are stored. |

## 13. Edge Cases and Test Cases

| Input | Expected output under the stated rule | Purpose |
| --- | --- | --- |
| [221, 321, 456, 1111] | [1111, 221, 321, 456] | Verifies lexicographical sorting of reversed strings with different lengths |
| [100, 2001, 1110, 321, 10101] | [100, 1110, 2001, 10101, 321] | Checks reversed-string ordering when reversed values include leading zeroes |
| [10, 1, 2] | [10, 1, 2] | Reversed strings 01, 1, 2 |
| [12, 21] | [21, 12] | Reversed-string ordering |
| [31, 13] | [31, 13] | Reversed strings 13 and 31 sort lexicographically in this order |
| [5] | [5] | Single value |

## 14. Common Mistakes

- Comparing reversed values numerically instead of lexicographically as strings.
- Sorting the reversed strings but returning those strings instead of the original numbers.
- Forgetting the original-number tie-break.

## 15. Pattern Identification

**Primary pattern:** Custom Comparator Sorting

**Why this pattern applies:** The primary sort key is a transformed string representation, with the original integer used as a secondary key.

## 16. Key Takeaways

- Keep each original number paired with its reversed digit string.
- Compare reversed strings lexicographically.
- Use the original number as the tie-breaker.
