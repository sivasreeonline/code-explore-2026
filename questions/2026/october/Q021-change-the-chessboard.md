# Q021: Change the Chessboard

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Combinatorics](https://img.shields.io/badge/topic-Combinatorics-blue) ![topic-Binomial%20Coefficients](https://img.shields.io/badge/topic-Binomial%20Coefficients-blue) ![topic-Modular%20Arithmetic](https://img.shields.io/badge/topic-Modular%20Arithmetic-blue) ![pattern-Combinatorics%20with%20Modular%20Exponentiation](https://img.shields.io/badge/pattern-Combinatorics%20with%20Modular%20Exponentiation-purple)

## 1. Problem Statement

Andrew is given a chessboard with R rows and C columns, and all its cells are currently black in color. He is assigned a task to change the color of any cell from black into white such that every single column of that chessboard contains an equal number of black and white cells.

Your task is to help Andrew to find and return the total number of ways he can change the chessboard in the above configuration.

**Note:** The result should be returned modulo 10^9 + 7.

## 2. Input Format

Parameters:

- input1: An integer value R, representing number of rows in the chessboard.
- input2: An integer value C, representing number of columns.

For standalone testing, line 1 contains R and line 2 contains C.

## 3. Output Format

Return an integer value denoting the total number of ways to change the chessboard in the above-mentioned configuration.

The result must be given modulo 10^9 + 7.

## 4. Constraints

- R and C are non-negative integers.
- Each column must contain the same number of black and white cells.
- All cells start black; changing a cell makes it white.
- Return the answer modulo 1,000,000,007.
- For even R, this factorial-based implementation requires R < 1,000,000,007.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 2
input2: 1
~~~

**Output**

~~~text
2
~~~

**Explanation**

There are two cells in the single column. Exactly one must be changed to white. Either the first cell or the second cell can be selected, giving 2 ways.

### Example 2

**Input**

~~~text
input1: 103
input2: 40
~~~

**Output**

~~~text
0
~~~

**Explanation**

Each column has 103 cells. Since 103 is odd, a column cannot contain equal numbers of black and white cells. Therefore there are 0 ways.

## 6. Intuition

Every column can be colored independently. If R is odd, no column can be split into equal black and white cells, so the answer is zero. If R is even, exactly R/2 cells in every column must be changed to white. There are choose(R, R/2) ways for one column, and that choice is repeated independently for each of the C columns.

## 7. Brute-Force Approach (When Relevant)

A brute-force approach could enumerate every coloring of the board and check each column. That is far too expensive.

## 8. Optimized Approach

**Core idea:** Count valid choices per column, then raise that count to the number of columns.

1. If R is odd, return 0.
2. Compute the central binomial coefficient, choose(R, R/2), modulo 10^9 + 7.
3. Raise that value to the power C using modular exponentiation.
4. Return the result.

This factorial-based implementation requires even R < 1,000,000,007. Its O(R) factorial loops are impractical for very large R.

## 9. Java Solution

The standalone program reads R and C, then prints the number of valid colorings. This is a complete standalone program.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    private static final long MOD = 1_000_000_007L;

    private static long modPow(long base, long exponent)
    {
        long result = 1;
        base %= MOD;

        while (exponent > 0)
        {
            if ((exponent & 1L) == 1L)
            {
                result = result * base % MOD;
            }
            base = base * base % MOD;
            exponent >>= 1;
        }

        return result;
    }

    private static long factorial(int n)
    {
        long result = 1;
        for (int i = 2; i <= n; i++)
        {
            result = result * i % MOD;
        }
        return result;
    }

    public static int changeChessboard(int r, int c)
    {
        if (r < 0 || c < 0)
        {
            throw new IllegalArgumentException("R and C must be non-negative.");
        }
        if ((r & 1) == 1)
        {
            return 0;
        }
        if (r >= MOD)
        {
            throw new IllegalArgumentException("This factorial implementation requires R to be smaller than the modulus.");
        }

        int half = r / 2;
        long factR = factorial(r);
        long factHalf = factorial(half);
        long denominator = factHalf * factHalf % MOD;
        long waysPerColumn = factR * modPow(denominator, MOD - 2) % MOD;

        return (int) modPow(waysPerColumn, c);
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] tokens = raw.split("\\s+");
        int r = Integer.parseInt(tokens[0]);
        int c = Integer.parseInt(tokens[1]);
        System.out.println(changeChessboard(r, c));
    }
}
~~~

## 10. Python Solution

The standalone program reads R and C, then prints the number of valid colorings. This is a complete standalone program.

~~~python
import sys

MOD = 1_000_000_007

def mod_pow(base, exponent):
    result = 1
    base %= MOD

    while exponent > 0:
        if exponent & 1:
            result = result * base % MOD
        base = base * base % MOD
        exponent >>= 1

    return result

def factorial(n):
    result = 1
    for value in range(2, n + 1):
        result = result * value % MOD
    return result

def change_chessboard(r, c):
    if r < 0 or c < 0:
        raise ValueError("R and C must be non-negative.")
    if r % 2 == 1:
        return 0
    if r >= MOD:
        raise ValueError("This factorial implementation requires R to be smaller than the modulus.")

    half = r // 2
    fact_r = factorial(r)
    fact_half = factorial(half)
    denominator = fact_half * fact_half % MOD
    ways_per_column = fact_r * pow(denominator, MOD - 2, MOD) % MOD
    return mod_pow(ways_per_column, c)

def main():
    values = list(map(int, sys.stdin.read().split()))
    if len(values) >= 2:
        print(change_chessboard(values[0], values[1]))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: R = 2, C = 1.

| Step | Calculation | Result |
| --- | --- | --- |
| 1 | R is even | Continue |
| 2 | Choose 1 white cell from 2 cells in the column | 2 ways |
| 3 | One column, so use the per-column count once | 2 |

Final answer: 2.

## 12. Time and Space Complexity

Let R be the row count and C be the column count.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(R + log C) | The factorials take O(R) time and modular exponentiation takes O(log C). |
| Auxiliary space | O(1) | Factorials are calculated iteratively without storing a table. |

## 13. Edge Cases and Test Cases

| R | C | Expected output | Purpose |
| --- | --- | --- | --- |
| 2 | 1 | 2 | Counts valid board arrangements for an even number of rows and one column |
| 103 | 40 | 0 | Returns zero when an odd number of rows makes the required balanced arrangement impossible |
| 4 | 1 | 6 | Choose any 2 of 4 cells in one column |
| 2 | 3 | 8 | Two choices per column, independently |
| 4 | 2 | 36 | Two columns independently choose 2 white cells out of 4 |

## 14. Common Mistakes

- Returning a nonzero value when R is odd.
- Counting the choices for the entire board as a sum instead of multiplying independent column choices.
- Forgetting the modulo operation.
- Computing factorial division with ordinary integer division instead of a modular inverse.
- Using factorials when R is too large for the stated implementation limit.

## 15. Pattern Identification

**Primary pattern:** Combinatorics with Modular Exponentiation

**Why this pattern applies:** Every column independently chooses exactly half of its cells to become white. The single-column count is a central binomial coefficient, and the independent choices multiply across C columns.

## 16. Key Takeaways

- If R is odd, the answer is 0.
- If R is even, every column must have R/2 white cells.
- Count one-column choices and raise that count to C modulo 10^9 + 7.
- For even R, the factorial-based implementation requires R < 1,000,000,007 and runs in O(R) time.
