# Q030: First Product Pair Found

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Nested%20Loops](https://img.shields.io/badge/topic-Nested%20Loops-blue) ![topic-Pair%20Search](https://img.shields.io/badge/topic-Pair%20Search-blue) ![pattern-First-Match%20Pair%20Search](https://img.shields.io/badge/pattern-First-Match%20Pair%20Search-purple)

## 1. Problem Statement

Scan i from left to right and j from i + 1 onward. Return the first pair whose product equals target, or an empty array if none exists.

## 2. Input Format

- n: Number of array elements.
- target: Required product.
- arr: Integer array.

For standalone programs, line 1 contains n and target; line 2 contains the n array values.

## 3. Output Format

Return the values of the first pair encountered in scan order whose product equals target. Return an empty array if no such pair exists.

## 4. Constraints

- n is non-negative, and arr contains n integers.
- Scan indices in increasing order with i < j.

## 5. Examples and Explanations

### Example 1

~~~text
arr = [2, 5, 3, 4]
target = 12
Output = [3, 4]
~~~

**Explanation**

The scan checks each i from left to right and then each j greater than i. The first match is 3 × 4 = 12, found at indices 2 and 3. The method returns immediately when it finds the pair.

## 6. Intuition

The required pair is not the pair with the smallest index sum. It is the first pair found by the specified nested-loop order. Return immediately when a match is encountered.

## 7. Brute-Force Approach (When Relevant)

Use two nested loops. For each i, inspect j values from i + 1 onward. Return the values as soon as their product matches target.

## 8. Optimized Approach

**Core idea:** Follow the required scan order and stop at the first valid pair.

1. Iterate i from 0 through n - 1.
2. For each i, iterate j from i + 1 through n - 1.
3. If arr[i] × arr[j] equals target, return the two values immediately.
4. If the loops finish without finding a match, return an empty array.

No index-sum comparison is needed in this variation.

## 9. Java Solution

Method signature: `findPair(int n, int target, int[] arr)`.

~~~java
import java.util.Scanner;
import java.util.Arrays;

public class Main
{
    public static int[] findPair(int n, int target, int[] arr)
    {
        for (int i = 0; i < n; i++)
        {
            for (int j = i + 1; j < n; j++)
            {
                if ((long) arr[i] * arr[j] == target)
                {
                    return new int[] {arr[i], arr[j]};
                }
            }
        }

        return new int[0];
    }

    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);
        if (!sc.hasNextInt())
        {
            sc.close();
            return;
        }

        int n = sc.nextInt();
        int target = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++)
        {
            arr[i] = sc.nextInt();
        }

        int[] result = findPair(n, target, arr);
        System.out.println(Arrays.toString(result));
        sc.close();
    }
}
~~~

## 10. Python Solution

~~~python
import sys

def findPair(n, target, arr):
    for i in range(min(n, len(arr))):
        for j in range(i + 1, min(n, len(arr))):
            if arr[i] * arr[j] == target:
                return [arr[i], arr[j]]
    return []

def main():
    values = list(map(int, sys.stdin.read().split()))
    if len(values) < 2:
        return

    n, target = values[0], values[1]
    arr = values[2:2 + n]
    print(findPair(n, target, arr))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

For arr = [2, 5, 3, 4] and target = 12:

| i | j | Values | Product | Action |
| --- | --- | --- | --- | --- |
| 0 | 1 | 2, 5 | 10 | Continue |
| 0 | 2 | 2, 3 | 6 | Continue |
| 0 | 3 | 2, 4 | 8 | Continue |
| 1 | 2 | 5, 3 | 15 | Continue |
| 1 | 3 | 5, 4 | 20 | Continue |
| 2 | 3 | 3, 4 | 12 | Return [3, 4] |

## 12. Time and Space Complexity

Let n be the array size.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(n²) worst case | The method may examine every pair before finding a match or returning an empty array. |
| Auxiliary space | O(1) | A constant number of indices are stored, excluding the returned pair. |

## 13. Edge Cases and Test Cases

| Array | Target | Expected output | Purpose |
| --- | --- | --- | --- |
| [2, 5, 3, 4] | 12 | [3, 4] | Returns the first matching product pair encountered by the nested-loop scan |
| [4, 2, 6, 3] | 12 | [4, 3] | Returns first pair in scan order, not Q027's minimum-index-sum tie-break |
| [1, 2, 3] | 100 | [] | No pair matches |
| [0, 0, 5] | 0 | [0, 0] | First zero-product pair |
| [2, 3, 4] | 8 | [2, 4] | A match at the end of the scan |

## 14. Common Mistakes

- Continuing the scan after the first match instead of returning immediately.
- Choosing the minimum index sum, which is the rule in Q027 but not in this variation.
- Checking pairs where i equals j.
- Returning indices instead of values.
- Forgetting to return an empty array when no pair is found.

## 15. Pattern Identification

**Primary pattern:** First-Match Pair Search

**Why this pattern applies:** Nested loops define the exact order in which pairs are considered, and the first matching product determines the answer.

## 16. Key Takeaways

- The order of nested loops is part of the problem requirement.
- Return immediately on the first product match.
- This variant is different from Q027, where all matches are compared using the index-sum rule.
