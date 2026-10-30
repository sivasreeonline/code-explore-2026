# Q027: Product Pair With Minimum Index Sum

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Nested%20Loops](https://img.shields.io/badge/topic-Nested%20Loops-blue) ![topic-Pair%20Search](https://img.shields.io/badge/topic-Pair%20Search-blue) ![topic-Tie-Breaking](https://img.shields.io/badge/topic-Tie-Breaking-blue) ![pattern-Brute-Force%20Pair%20Enumeration%20with%20Lexicographic%20Tie-Breaking](https://img.shields.io/badge/pattern-Brute-Force%20Pair%20Enumeration%20with%20Lexicographic%20Tie-Breaking-purple)

## 1. Problem Statement

Find indices i and j where i is less than j and arr[i] multiplied by arr[j] equals target. Choose the pair with the smallest sum of indices i + j; tie-break by smaller i, then smaller j. Return the pair's values or an empty array if no pair exists.

## 2. Input Format

- n: Number of array elements.
- target: Required product.
- arr: Integer array.

For standalone programs, line 1 contains n and target; line 2 contains the n array values.

## 3. Output Format

Return a two-element array containing the values at the chosen indices. Return an empty array if no valid pair exists.

## 4. Constraints

- n is non-negative, and arr contains n integers.
- Indices are zero-based and must satisfy i < j.

## 5. Examples and Explanations

### Example 1

~~~text
arr = [4, 2, 6, 3]
target = 12
Output = [4, 3]
~~~

The pairs at indices (0, 3) and (1, 2) both multiply to 12 and both have index sum 3. The tie-break chooses the smaller first index, so the returned values are [4, 3].

## 6. Intuition

Several pairs may have the required product. Finding any pair is not enough: examine all valid pairs and compare their index sums. If index sums tie, prefer the pair with the smaller first index, then the smaller second index.

## 7. Brute-Force Approach (When Relevant)

Use two nested loops. The first loop chooses i, and the second chooses every j greater than i. When a pair's product equals target, compare its index tuple against the best pair found so far.

## 8. Optimized Approach

**Core idea:** Enumerate all pairs and keep the best according to the required ordering.

1. Initialize the best pair as absent.
2. For each i from 0 to n - 1, iterate j from i + 1 to n - 1.
3. Compare the product using a wide integer type in Java to reduce overflow risk.
4. If the product matches target, prefer the pair with smaller i + j, then smaller i, then smaller j.
5. Return the selected values, or an empty array.

Check every pair in index order. This takes O(N²) time and guarantees that no better index sum is missed.

## 9. Java Solution

~~~java
import java.util.Scanner;
import java.util.Arrays;

public class Main
{
    public static int[] findPair(int n, int target, int[] arr)
    {
        int bestI = -1;
        int bestJ = -1;
        int bestSum = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++)
        {
            for (int j = i + 1; j < n; j++)
            {
                if ((long) arr[i] * arr[j] != target)
                {
                    continue;
                }

                int sum = i + j;
                if (sum < bestSum
                    || (sum == bestSum && (bestI == -1
                    || i < bestI
                    || (i == bestI && j < bestJ))))
                {
                    bestSum = sum;
                    bestI = i;
                    bestJ = j;
                }
            }
        }

        if (bestI == -1)
        {
            return new int[0];
        }

        return new int[] {arr[bestI], arr[bestJ]};
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
    best_pair = None
    best_key = None

    for i in range(min(n, len(arr))):
        for j in range(i + 1, min(n, len(arr))):
            if arr[i] * arr[j] != target:
                continue

            key = (i + j, i, j)
            if best_key is None or key < best_key:
                best_key = key
                best_pair = [arr[i], arr[j]]

    return best_pair if best_pair is not None else []

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

For arr = [4, 2, 6, 3] and target = 12:

| i | j | Values | Product | Index Sum | Decision |
| --- | --- | --- | --- | --- | --- |
| 0 | 2 | 4, 6 | 24 | 2 | Not a match |
| 0 | 3 | 4, 3 | 12 | 3 | Best pair so far |
| 1 | 2 | 2, 6 | 12 | 3 | Ties sum; keep smaller i = 0 |
| 1 | 3 | 2, 3 | 6 | 4 | Not a match |
| 2 | 3 | 6, 3 | 18 | 5 | Not a match |

Result: [4, 3].

## 12. Time and Space Complexity

Let n be the number of values.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(n²) | Every pair with i < j may be inspected. |
| Auxiliary space | O(1) | Only the best indices and comparison values are stored, excluding the returned pair. |

## 13. Edge Cases and Test Cases

| Array | Target | Expected output | Purpose |
| --- | --- | --- | --- |
| [4, 2, 6, 3] | 12 | [4, 3] | Checks tie-breaking between matching pairs with equal index sums |
| [2, 5, 3, 4] | 12 | [3, 4] | Only the final pair matches |
| [1, 2, 3] | 100 | [] | No matching pair |
| [0, 0, 1] | 0 | [0, 0] | Zero product |

| Array | Target | Expected output | Purpose |
| --- | --- | --- | --- |
| [-2, 3, 4, -6] | 12 | [-2, -6] | Negative values and index-sum tie-break |

## 14. Common Mistakes

- Returning the first product match instead of the pair with minimum index sum.
- Comparing only index sums and omitting the tie-breaks.
- Using i greater than j or allowing the same index twice.
- Returning indices when the requirement asks for the pair's values.
- Multiplying Java integers without widening when large values are possible.

## 15. Pattern Identification

**Primary pattern:** Brute-Force Pair Enumeration with Lexicographic Tie-Breaking

**Why this pattern applies:** The answer depends on evaluating valid index pairs and choosing the minimum tuple (i + j, i, j).

## 16. Key Takeaways

- Pair validity depends on the product.
- Pair selection depends on the index-sum and index tie-break rules.
- Return values in index order, not the indices themselves.
