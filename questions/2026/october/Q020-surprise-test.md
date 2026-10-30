# Q020: Surprise Test

![difficulty-Hard](https://img.shields.io/badge/difficulty-Hard-red) ![topic-Number%20Theory](https://img.shields.io/badge/topic-Number%20Theory-blue) ![topic-Perfect%20Powers](https://img.shields.io/badge/topic-Perfect%20Powers-blue) ![topic-Inclusion-Exclusion](https://img.shields.io/badge/topic-Inclusion-Exclusion-blue) ![topic-Integer%20Roots](https://img.shields.io/badge/topic-Integer%20Roots-blue) ![pattern-Number%20Theory%20with%20Inclusion-Exclusion](https://img.shields.io/badge/pattern-Number%20Theory%20with%20Inclusion-Exclusion-purple)

## 1. Problem Statement

Philip is taking a surprise test. The problem in the test includes a range of integers from 1 to N (inclusive) where the candidates need to find the count of the Special Numbers in the range. The teacher describes Special Numbers as numbers that cannot be written in the form of c^d, where c and d are positive integers greater than 1.

Your task is to help Philip to find and return a string value representing the count of Special Numbers in the given range of 1 to N.

## 2. Input Format

Parameters:

- input1: A string value N, representing the upper limit of the range of numbers that needs to be checked for Special Numbers.

For standalone testing, line 1 contains N as a decimal string.

## 3. Output Format

Return a string representing the count of Special Numbers in the given range from 1 to N.

## 4. Constraints

- N is a non-negative integer supplied as a decimal string.
- The range includes every integer from 1 through N.
- 1 is special.
- A number is special if it cannot be written as c^d for integers c > 1 and d > 1.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 8
~~~

**Output**

~~~text
6
~~~

**Explanation**

The numbers 4 and 8 can be represented as 2^2 and 2^3, respectively, so they are not special. The remaining numbers 1, 2, 3, 5, 6, and 7 are special. The count is 6.

### Example 2

**Input**

~~~text
input1: 11
~~~

**Output**

~~~text
8
~~~

**Explanation**

The numbers 4, 8, and 9 can be represented as 2^2, 2^3, and 3^2. The remaining numbers 1, 2, 3, 5, 6, 7, 10, and 11 are special. The count is 8.

## 6. Intuition

Count the numbers that are perfect powers, then subtract that count from N. A perfect power may have multiple representations, such as 64 being both a square and a cube, so counting all powers separately would overcount. Inclusion-exclusion over square-free exponents counts each perfect-power value exactly once.

## 7. Brute-Force Approach (When Relevant)

A direct approach could generate every power c^d up to N, store the distinct results, and subtract their count from N. This becomes impractical for large N and needs a set to remove duplicates.

## 8. Optimized Approach

**Core idea:** Count the union of all p-th powers where p is prime, using inclusion-exclusion.

1. Let the maximum relevant exponent be the largest integer e for which 2^e is at most N.
2. Compute the Möbius value for each exponent up to that maximum.
3. For each square-free exponent e greater than 1, find the exact integer floor of the e-th root of N.
4. Add or subtract root(N,e) - 1 according to the sign opposite to the Möbius value.
5. Subtract the resulting count of distinct perfect powers from N.
6. Return the result as a decimal string.

The integer-root calculation uses integer comparisons rather than floating-point rounding.

## 9. Java Solution

This standalone program uses `BigInteger` so N can exceed the 32-bit integer range. This standalone program uses BigInteger so the input string is not restricted to a 32-bit integer.

~~~java
import java.io.*;
import java.math.BigInteger;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;

public class Main
{
    private static int[] mobiusValues(int limit)
    {
        int[] mu = new int[limit + 1];
        boolean[] composite = new boolean[limit + 1];
        List<Integer> primes = new ArrayList<>();

        if (limit >= 1)
        {
            mu[1] = 1;
        }

        for (int i = 2; i <= limit; i++)
        {
            if (!composite[i])
            {
                primes.add(i);
                mu[i] = -1;
            }

            for (int p : primes)
            {
                long product = (long) i * p;
                if (product > limit)
                {
                    break;
                }

                composite[(int) product] = true;
                if (i % p == 0)
                {
                    mu[(int) product] = 0;
                    break;
                }
                mu[(int) product] = -mu[i];
            }
        }

        return mu;
    }

    private static BigInteger integerNthRoot(BigInteger n, int exponent)
    {
        if (n.compareTo(BigInteger.ONE) <= 0)
        {
            return n;
        }

        int highBits = (n.bitLength() + exponent - 1) / exponent;
        BigInteger low = BigInteger.ONE;
        BigInteger high = BigInteger.ONE.shiftLeft(highBits);

        while (high.subtract(low).compareTo(BigInteger.ONE) > 0)
        {
            BigInteger mid = low.add(high).shiftRight(1);
            if (mid.pow(exponent).compareTo(n) <= 0)
            {
                low = mid;
            }
            else
            {
                high = mid;
            }
        }

        return low;
    }

    public static String countSpecialNumbers(String input)
    {
        BigInteger n = new BigInteger(input.trim());
        if (n.signum() <= 0)
        {
            return "0";
        }

        int maxExponent = n.bitLength() - 1;
        int[] mu = mobiusValues(maxExponent);
        BigInteger perfectPowerCount = BigInteger.ZERO;

        for (int exponent = 2; exponent <= maxExponent; exponent++)
        {
            if (mu[exponent] == 0)
            {
                continue;
            }

            BigInteger rootCount = integerNthRoot(n, exponent).subtract(BigInteger.ONE);
            if (mu[exponent] < 0)
            {
                perfectPowerCount = perfectPowerCount.add(rootCount);
            }
            else
            {
                perfectPowerCount = perfectPowerCount.subtract(rootCount);
            }
        }

        return n.subtract(perfectPowerCount).toString();
    }

    public static void main(String[] args) throws Exception
    {
        String input = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (!input.isEmpty())
        {
            System.out.println(countSpecialNumbers(input));
        }
    }
}
~~~

## 10. Python Solution

This standalone program uses `BigInteger` so N can exceed the 32-bit integer range. This standalone program uses Python's arbitrary-precision integers.

~~~python
import sys

def mobius_values(limit):
    mu = [0] * (limit + 1)
    composite = [False] * (limit + 1)
    primes = []

    if limit >= 1:
        mu[1] = 1

    for value in range(2, limit + 1):
        if not composite[value]:
            primes.append(value)
            mu[value] = -1

        for prime in primes:
            product = value * prime
            if product > limit:
                break
            composite[product] = True
            if value % prime == 0:
                mu[product] = 0
                break
            mu[product] = -mu[value]

    return mu

def integer_nth_root(n, exponent):
    if n <= 1:
        return n

    low = 1
    high = 1 << ((n.bit_length() + exponent - 1) // exponent)

    while high - low > 1:
        middle = (low + high) // 2
        if middle ** exponent <= n:
            low = middle
        else:
            high = middle

    return low

def count_special_numbers(value):
    n = int(value.strip())
    if n <= 0:
        return "0"

    max_exponent = n.bit_length() - 1
    mu = mobius_values(max_exponent)
    perfect_power_count = 0

    for exponent in range(2, max_exponent + 1):
        if mu[exponent] == 0:
            continue

        root_count = integer_nth_root(n, exponent) - 1
        perfect_power_count += -mu[exponent] * root_count

    return str(n - perfect_power_count)

def main():
    value = sys.stdin.read().strip()
    if value:
        print(count_special_numbers(value))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

For N = 11, the relevant perfect powers greater than 1 are 4, 8, and 9.

| Number | Perfect power? | Count as special? |
| --- | --- | --- |
| 1 | No | Yes |
| 2 | No | Yes |
| 3 | No | Yes |
| 4 | Yes: 2^2 | No |
| 5 | No | Yes |
| 6 | No | Yes |
| 7 | No | Yes |
| 8 | Yes: 2^3 | No |
| 9 | Yes: 3^2 | No |
| 10 | No | Yes |
| 11 | No | Yes |

There are 3 perfect powers and 8 special numbers.

## 12. Time and Space Complexity

Let B be the bit length of N.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | Depends on B and the exact integer-root searches | The algorithm sieves exponents and computes integer roots for square-free exponents up to B - 1. |
| Auxiliary space | O(B) exponent storage, excluding BigInteger arithmetic | Möbius values and the sieve arrays are proportional to the maximum exponent. |

## 13. Edge Cases and Test Cases

| N | Expected output | Purpose |
| --- | --- | --- |
| 8 | 6 | Counts the values up to 8 after excluding perfect powers |
| 11 | 8 | Counts valid values up to 11 while excluding repeated perfect-power cases |
| 1 | 1 | One itself is special |
| 2 | 2 | No perfect powers greater than 1 |
| 4 | 3 | Excludes only 4 |
| 9 | 7 | Excludes 4, 8, and 9 |
| 64 | 54 | Ten distinct perfect powers in the range, with overlaps counted only once |

## 14. Common Mistakes

- Counting 1 as a perfect power even though both c and d must be greater than 1.
- Counting overlapping representations multiple times, such as 64 = 2^6 = 4^3 = 8^2.
- Treating every number that is not prime as a perfect power.
- Using floating-point roots without exact integer correction.
- Assuming N fits in a standard 32-bit integer despite the string input specification.

## 15. Pattern Identification

**Primary pattern:** Number Theory with Inclusion-Exclusion

**Why this pattern applies:** Perfect powers form a union of sets of prime-exponent powers. Inclusion-exclusion corrects overlap between these sets, allowing the distinct perfect-power count to be subtracted from N.

## 16. Key Takeaways

- A special number cannot be written as c^d with both c and d greater than 1.
- Count distinct perfect-power values, not their representations.
- Integer roots and inclusion-exclusion avoid enumerating every integer up to N.
