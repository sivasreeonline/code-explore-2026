# Q024: Mixed Fraction

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Enumeration](https://img.shields.io/badge/topic-Enumeration-blue) ![topic-Arithmetic](https://img.shields.io/badge/topic-Arithmetic-blue) ![topic-Fractions](https://img.shields.io/badge/topic-Fractions-blue) ![topic-Exact%20Comparison](https://img.shields.io/badge/topic-Exact%20Comparison-blue) ![topic-Lexicographical%20Sorting](https://img.shields.io/badge/topic-Lexicographical%20Sorting-blue) ![pattern-Exhaustive%20Enumeration%20with%20Exact%20Arithmetic](https://img.shields.io/badge/pattern-Exhaustive%20Enumeration%20with%20Exact%20Arithmetic-purple)

## 1. Problem Statement

**Mixed Fraction**

You are given four distinct integers. Choose an ordered triple of distinct values a, b, and c, and calculate a + b/c. Let d be the unused value. For each v in {a, b, c}, form the fractions d/v and v/d. A fraction is valid when it equals a + b/c. Return each valid fraction once, sorted lexicographically and separated by spaces. Denominators must be nonzero.

## 2. Input Format

**Parameters:**

- input1: An integer value representing the first distinct number.
- input2: An integer representing the second distinct number.
- input3: An integer representing the third distinct number.
- input4: An integer representing the fourth distinct number.

**Standalone input convention:** Read four integers separated by whitespace; they may be entered one per line.

The standalone programs read the four integers in input order.

## 3. Output Format

Return a single string containing all valid fractions x/y separated by spaces and sorted lexicographically. The standalone programs print the resulting string on one line.

## 4. Constraints

- The four input integers are distinct.
- Choose an ordered triple of distinct values a, b, and c; the fourth value is d.
- c and every fraction denominator must be nonzero.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 15
input2: 7
input3: 2
input4: 1
~~~

**Output**

~~~text
15/2 15/7
~~~

**Explanation**

The integers are 15, 7, 2, and 1.

- Choose a = 7, b = 1, c = 2. Then 7 + (1/2) = 15/2, giving 15/2.
- Choose a = 2, b = 1, c = 7. Then 2 + (1/7) = 15/7, giving 15/7.

Sorting the fraction strings lexicographically gives 15/2 15/7.

### Example 2

**Input**

~~~text
input1: 1
input2: 2
input3: 3
input4: 5
~~~

**Output**

~~~text
5/1 5/2 5/3
~~~

**Explanation**

The integers are 1, 2, 3, and 5.

- Choose a = 2, b = 3, c = 1. Then 2 + (3/1) = 5, giving 5/1.
- Choose a = 1, b = 3, c = 2. Then 1 + (3/2) = 5/2, giving 5/2.
- Choose a = 1, b = 2, c = 3. Then 1 + (2/3) = 5/3, giving 5/3.

Sorting the fraction strings lexicographically gives 5/1 5/2 5/3.

## 6. Intuition

Try each valid ordered choice of a, b, and c. The fourth value is the one not used in that expression. Pair it with each of the three selected values in both numerator/denominator orders, then check whether the fraction equals a + b/c. Keep valid fractions, remove duplicates, and sort the result.

## 7. Brute-Force Approach (When Relevant)

There are only four input values, so exhaustive enumeration is small.

1. Choose three distinct input positions for a, b, and c.
2. Identify the one unused input position.
3. For each selected position, form both candidate fractions: unused/reused and reused/unused.
4. Skip candidates with c = 0 or y = 0.
5. Check equality using exact integer arithmetic.
6. Store valid fraction strings in a set and return them in lexicographical order.

## 8. Optimized Approach

**Core idea:** Exhaustively check the small set of possibilities, while using exact arithmetic instead of floating-point division.

Since

~~~text
a + (b / c) = (a * c + b) / c
~~~

the candidate fraction x/y is valid exactly when

~~~text
(a * c + b) * y = x * c
~~~

This avoids floating-point rounding errors.

1. Enumerate every ordered triple of distinct input positions for a, b, and c.
2. Find the one unused input position.
3. Pair that unused number with each previously selected number, checking both possible fraction orders.
4. Use the cross-multiplication equality to check validity.
5. Store valid fractions in a sorted set and join them using one space.

## 9. Java Solution

The Java solution uses BigInteger for exact arithmetic and to avoid overflow in intermediate cross products.

~~~java
import java.math.BigInteger;
import java.util.Scanner;
import java.util.TreeSet;

public class Main
{
    private static void checkFraction(
        BigInteger a,
        BigInteger b,
        BigInteger c,
        BigInteger x,
        BigInteger y,
        TreeSet<String> result)
    {
        if (c.signum() == 0 || y.signum() == 0)
        {
            return;
        }

        BigInteger left = a.multiply(c).add(b).multiply(y);
        BigInteger right = x.multiply(c);

        if (left.equals(right))
        {
            result.add(x.toString() + "/" + y.toString());
        }
    }

    private static String mixedFraction(BigInteger[] nums)
    {
        TreeSet<String> result = new TreeSet<>();

        for (int i = 0; i < 4; i++)
        {
            for (int j = 0; j < 4; j++)
            {
                for (int k = 0; k < 4; k++)
                {
                    if (i == j || i == k || j == k)
                    {
                        continue;
                    }

                    int unused = 0;
                    while (unused == i || unused == j || unused == k)
                    {
                        unused++;
                    }

                    BigInteger a = nums[i];
                    BigInteger b = nums[j];
                    BigInteger c = nums[k];

                    if (c.signum() == 0)
                    {
                        continue;
                    }

                    for (int reused : new int[] {i, j, k})
                    {
                        checkFraction(a, b, c, nums[unused], nums[reused], result);
                        checkFraction(a, b, c, nums[reused], nums[unused], result);
                    }
                }
            }
        }

        return String.join(" ", result);
    }

    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);
        BigInteger[] nums = new BigInteger[4];

        for (int i = 0; i < 4; i++)
        {
            nums[i] = sc.nextBigInteger();
        }

        System.out.println(mixedFraction(nums));
        sc.close();
    }
}
~~~

## 10. Python Solution

Python integers support arbitrary-precision arithmetic, so the same exact cross-multiplication check works directly.

~~~python
from itertools import permutations
import sys

def mixed_fraction(nums):
    results = set()

    for i, j, k in permutations(range(4), 3):
        a, b, c = nums[i], nums[j], nums[k]

        if c == 0:
            continue

        unused = next(index for index in range(4) if index not in (i, j, k))

        for reused in (i, j, k):
            candidates = (
                (nums[unused], nums[reused]),
                (nums[reused], nums[unused]),
            )

            for x, y in candidates:
                if y == 0:
                    continue

                if (a * c + b) * y == x * c:
                    results.add(f"{x}/{y}")

    return " ".join(sorted(results))

def main():
    values = list(map(int, sys.stdin.read().split()))
    if len(values) != 4:
        raise ValueError("Expected exactly four integers.")
    print(mixed_fraction(values))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

For input values [1, 2, 3, 5]:

| a | b | c | Calculation | Unused value | Valid fraction |
| ---: | ---: | ---: | --- | ---: | --- |
| 2 | 3 | 1 | 2 + 3/1 = 5 | 5 | 5/1 |
| 1 | 3 | 2 | 1 + 3/2 = 5/2 | 5 | 5/2 |
| 1 | 2 | 3 | 1 + 2/3 = 5/3 | 5 | 5/3 |

The unique fraction strings are sorted lexicographically, producing 5/1 5/2 5/3.

## 12. Time and Space Complexity

There are exactly four input numbers, so the number of candidate combinations is fixed.

| Complexity | Result | Reason |
| --- | --- | --- |
| Candidate checks | O(1) for this fixed-size problem | Only ordered triples from four positions and a small set of fraction pairings are evaluated. |
| Sorting | O(K log K) | K is the number of unique valid fraction strings. |
| Auxiliary space | O(K) | The set stores the unique valid output fractions. |

## 13. Edge Cases and Test Cases

| Input values | Expected output | Purpose |
| --- | --- | --- |
| 15, 7, 2, 1 | 15/2 15/7 | Finds two valid fractions and returns them in lexicographical order |
| 1, 2, 3, 5 | 5/1 5/2 5/3 | Finds three valid fractions and verifies lexicographical ordering |
| 1, 2, 3, 4 | 3/1 3/2 | Checks two valid fractions formed from the unused value |
| 0, 2, 3, 5 | "" (empty output) | Confirms zero-denominator candidates are skipped and no fraction satisfies the equality |

## 14. Common Mistakes

- Comparing fractions with floating-point division, which can misclassify exact equality.
- Allowing c = 0 or y = 0, which makes the expression or candidate fraction undefined.
- Reusing an input value by position incorrectly or omitting one valid numerator/denominator order.

## 15. Pattern Identification

**Primary pattern:** Exhaustive Enumeration with Exact Arithmetic

The input contains only four integers, so enumerating all permitted assignments is practical. Cross multiplication avoids precision errors from floating-point division, while a set removes duplicate results and sorts the output lexicographically.

## 16. Key Takeaways

- Preserve the order of a, b, and c because a + b/c is not commutative.
- Do not use integer division for b/c.
- Use cross multiplication to compare fractions exactly.
- Skip candidates that divide by zero.
- Deduplicate and sort fraction strings lexicographically.
- Pair the unused value with each selected value in both orders to form candidate fractions.
