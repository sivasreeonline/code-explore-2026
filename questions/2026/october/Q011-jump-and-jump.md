# Q011: Jump and Jump

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Mathematics](https://img.shields.io/badge/topic-Mathematics-blue) ![topic-Greedy%20Optimization](https://img.shields.io/badge/topic-Greedy%20Optimization-blue) ![topic-Integer%20Arithmetic](https://img.shields.io/badge/topic-Integer%20Arithmetic-blue) ![pattern-Mathematical%20Optimization](https://img.shields.io/badge/pattern-Mathematical%20Optimization-purple)

## 1. Problem Statement

**Jump and Jump**

You start at point P on a number line and make exactly X jumps. Each jump moves you forward by L or backward by L. Choose the directions to make the absolute value of your final position as small as possible.

Your task is to find and return an integer value representing the minimum possible absolute value of the point you can reach after performing exactly X jumps.

The origin is 0. Choose the direction of each jump independently.

## 2. Input Format

Parameters:

- input1: An integer value P, representing your initial position on the number line.
- input2: An integer value X, representing the total number of jumps to perform.
- input3: An integer value L, representing the length of each jump.

The Java and Python programs are standalone implementations.

For standalone input:

- Line 1: P
- Line 2: X
- Line 3: L

## 3. Output Format

Return an integer value representing the minimum possible absolute value of the point reachable after performing exactly X jumps.

The standalone programs print the minimum absolute value on one line.

## 4. Constraints

- X is non-negative.
- L is a positive integer.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1 (P): 7
input2 (X): 4
input3 (L): 3
~~~

**Output**

~~~text
1
~~~

**Explanation**

Starting from P = 7 with four jumps of length 3, one possible path is:

- 7 - 3 = 4 (backward)
- 4 + 3 = 7 (forward)
- 7 - 3 = 4 (backward)
- 4 - 3 = 1 (backward)

The minimum absolute value reachable after exactly four jumps is 1.

### Example 2

**Input**

~~~text
input1 (P): 6
input2 (X): 2
input3 (L): 4
~~~

**Output**

~~~text
2
~~~

**Explanation**

- Start at P = 6.
- First jump backward: 6 - 4 = 2.
- Second jump backward: 2 - 4 = -2.

The absolute value of the final point is 2, which is the minimum possible.

## 6. Intuition

Each jump changes the position by either plus L or minus L. The exact sequence of directions matters only through how many jumps are forward and how many are backward. Since there are exactly X jumps, there are only X + 1 possible net direction balances to consider. We can derive the best balance mathematically instead of trying every combination of directions.

## 7. Brute-Force Approach (When Relevant)

Try every possible number k of forward jumps from 0 through X. The remaining X - k jumps are backward. For each k, calculate the final position P + (2k - X) * L and compare its absolute value with the best result found so far. This examines X + 1 candidates; it does not need to enumerate all possible orders of the jumps.

## 8. Optimized Approach

**Core idea:** The final position for k forward jumps is P + (2k - X) * L. This value changes linearly with k, so the minimum absolute value occurs at one of the integers closest to the real-valued target.

1. Compute numerator = X * L - P and denominator = 2 * L.
2. Compute the floor of numerator divided by denominator.
3. Clamp that candidate k to the range from 0 through X.
4. Also test the next integer candidate, clamped to the same range.
5. Calculate the final position for both candidates and return the smaller absolute value.

The Java implementation uses floor division for negative numerators. The Python implementation uses integer floor division, which has the same floor behavior. Intermediate arithmetic uses a wider integer type where available.

## 9. Java Solution

This standalone implementation includes a helper method and input driver.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    private static long finalPosition(long p, long x, long length, long forwardJumps)
    {
        return p + (2 * forwardJumps - x) * length;
    }

    public static int minimumDistance(int p, int x, int length)
    {
        if (x < 0 || length < 0)
        {
            throw new IllegalArgumentException("X must be non-negative and L must not be negative.");
        }

        if (length == 0)
        {
            return Math.toIntExact(Math.abs((long) p));
        }

        long numerator = (long) x * length - p;
        long denominator = 2L * length;
        long floorCandidate = Math.floorDiv(numerator, denominator);

        long k1 = Math.max(0L, Math.min((long) x, floorCandidate));
        long k2 = Math.max(0L, Math.min((long) x, floorCandidate + 1));

        long first = Math.abs(finalPosition(p, x, length, k1));
        long second = Math.abs(finalPosition(p, x, length, k2));

        return Math.toIntExact(Math.min(first, second));
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] values = raw.split("\\s+");
        int p = Integer.parseInt(values[0]);
        int x = Integer.parseInt(values[1]);
        int length = Integer.parseInt(values[2]);

        System.out.println(minimumDistance(p, x, length));
    }
}
~~~

## 10. Python Solution

The Java method preserves the platform class, method, parameters, and return type.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def minimumDistance(cls, input1, input2, input3):
        '''
        input1 : int
        input2 : int
        input3 : int
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        p, x, length = input1, input2, input3

        if x < 0 or length < 0:
            raise ValueError("X must be non-negative and L must not be negative.")

        if length == 0:
            return abs(p)

        denominator = 2 * length
        floor_candidate = (x * length - p) // denominator

        k1 = max(0, min(x, floor_candidate))
        k2 = max(0, min(x, floor_candidate + 1))

        position1 = p + (2 * k1 - x) * length
        position2 = p + (2 * k2 - x) * length

        return min(abs(position1), abs(position2))
~~~

## 11. Dry Run

Example 2: P = 6, X = 2, L = 4.

| Forward Jumps k | Backward Jumps X - k | Final Position | Absolute Value |
| --- | --- | --- | --- |
| 0 | 2 | 6 + (0 - 2) * 4 = -2 | 2 |
| 1 | 1 | 6 + (2 - 2) * 4 = 6 | 6 |
| 2 | 0 | 6 + (4 - 2) * 4 = 14 | 14 |

The minimum absolute value is 2. The algorithm computes the nearest integer candidate around the real-valued target and checks both adjacent integer choices, including k = 0 in this example.

## 12. Time and Space Complexity

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(1) | Only two integer candidates are evaluated. |
| Auxiliary space | O(1) | A fixed number of integer variables are used. |

## 13. Edge Cases and Test Cases

| P | X | L | Expected Output | Purpose |
| --- | --- | --- | --- | --- |
| 7 | 4 | 3 | 1 | Checks the minimum-distance calculation for a case requiring one jump |
| 6 | 2 | 4 | 2 | Checks minimum-distance calculation for a different starting value and jump range |
| 0 | 2 | 3 | 0 | The origin is reachable |
| -7 | 3 | 2 | 1 | Negative starting position |
| 10 | 0 | 3 | 10 | No jumps are performed |
| 5 | 2 | 0 | 5 | Zero-length jumps, handled as a degenerate case |
| 7 | 3 | 2 | 1 | Checks the result for a second jump-size and range combination |
| 10 | 2 | 6 | 2 | Checks the result for a second jump-size and range combination |

## 14. Common Mistakes

- Minimizing the signed final position instead of its absolute value.
- Allowing more or fewer than X jumps.
- Assuming all jumps must be in the same direction.
- Checking only one rounded candidate instead of checking the neighboring integer candidate.
- Using integer division that truncates toward zero instead of floor division for a negative numerator.

## 15. Pattern Identification

**Primary pattern:** Mathematical Optimization

**Why this pattern applies:** Each possible number of forward jumps determines a linearly spaced final position. Minimizing the absolute value reduces to checking the integer candidates closest to the target balance, with the candidate range clamped to 0 through X.

## 16. Key Takeaways

- With exactly X jumps, k forward jumps imply X - k backward jumps.
- The final position is P + (2k - X) * L.
- The optimum can be found from two neighboring integer candidates in O(1) time.
