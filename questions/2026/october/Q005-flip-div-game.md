# Q005: Flip Div Game

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Bit%20Manipulation](https://img.shields.io/badge/topic-Bit%20Manipulation-blue) ![topic-XOR](https://img.shields.io/badge/topic-XOR-blue) ![topic-Bit%20Indexing](https://img.shields.io/badge/topic-Bit%20Indexing-blue) ![pattern-Bit%20Manipulation%20and%20Exhaustive%20Search](https://img.shields.io/badge/pattern-Bit%20Manipulation%20and%20Exhaustive%20Search-purple)

## 1. Problem Statement

**Flip Div Game**

Andy is playing the Flip Div Game, where he has two integers N and K. Initially, N is not divisible by K. He must flip exactly one bit of N so that K can divide N. Your task is to help Andy find and return an integer value representing the minimum bit index that needs to be flipped to achieve this divisibility. In case it is not possible, return -1.

**Note:**

- Consider the number N to be a 32-bit integer.
- Bit positions are 1-based.
- The rightmost bit is considered at the 1st index, the 2nd rightmost bit is considered at the 2nd index and so on.

## 2. Input Format

**Parameters:**

- input1: An integer value, representing the number N.
- input2: An integer value, representing the number K.

**Standalone program input convention used by the solutions below:**

- Line 1: Integer N.
- Line 2: Integer K.

Platform method: flipBit(cls, input1, input2), returning an integer. Treat N as a signed 32-bit two's-complement value.

## 3. Output Format

Return an integer value representing the minimum bit index that needs to be flipped to achieve the divisibility of N by K. In case it is not possible, return -1.

For the standalone programs, print the bit index or -1 on one line.

## 4. Constraints

- N is a signed 32-bit integer.
- K is a positive integer.
- N is not divisible by K initially.
- Bit positions are numbered 1 through 32, from right to left.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 3
input2: 5
~~~

**Output**

~~~text
6
~~~

**Explanation**

N = 3 has binary representation 11. Check bit positions from right to left:

| Bit Index | Value After Flipping That Bit | Divisible by 5? |
| --- | --- | --- |
| 1 | 2 | No |
| 2 | 1 | No |
| 3 | 7 | No |
| 4 | 11 | No |
| 5 | 19 | No |
| 6 | 35 | Yes |

The first bit index that makes the resulting value divisible by 5 is 6.

**Standalone input for the programs below**

~~~text
3
5
~~~

### Example 2

**Input**

~~~text
input1: 3
input2: 10
~~~

**Output**

~~~text
-1
~~~

**Explanation**

None of the 32 bit flips makes N divisible by 10, so the answer is -1.

**Standalone input for the programs below**

~~~text
3
10
~~~

## 6. Intuition

Flipping a bit changes exactly one binary position. For each position, toggle that bit and check whether the resulting integer is divisible by K. Since the question asks for the minimum 1-based bit index, test positions from the rightmost bit to the leftmost bit and stop at the first match.

## 7. Brute-Force Approach (When Relevant)

Try flipping each of the 32 bits one at a time. For every candidate number, check divisibility by K. Return the first successful bit index; return -1 if all 32 candidates fail.

## 8. Optimized Approach

**Core idea:** Use XOR to toggle one bit and scan bit positions in increasing index order.

1. Start with bit index 1, corresponding to bit position 0 in zero-based programming notation.
2. Toggle that bit with XOR.
3. Check whether the candidate number is divisible by K.
4. If it is divisible, return the current 1-based bit index.
5. Otherwise, continue through bit index 32.
6. If no candidate is divisible by K, return -1.

N has 32 bits, so checking every position takes constant time.

## 9. Java Solution

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    public static int flipBit(int n, int k)
    {
        if (k == 0)
        {
            return -1;
        }

        for (int bit = 0; bit < 32; bit++)
        {
            int candidate = n ^ (1 << bit);
            if (candidate % k == 0)
            {
                return bit + 1;
            }
        }
        return -1;
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] values = raw.split("\\s+");
        int n = Integer.parseInt(values[0]);
        int k = Integer.parseInt(values[1]);
        System.out.println(flipBit(n, k));
    }
}
~~~

## 10. Python Solution

Platform method: flipBit(cls, input1, input2), returning an integer. Treat N as a signed 32-bit two's-complement value.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def flipBit(cls, input1, input2):
        '''
        input1 : int
        input2 : int
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        n = input1
        k = input2

        if k == 0:
            return -1

        for bit in range(32):
            candidate = (n ^ (1 << bit)) & 0xFFFFFFFF
            if candidate >= (1 << 31):
                candidate -= 1 << 32

            if candidate % k == 0:
                return bit + 1

        return -1
~~~

## 11. Dry Run

Example 1: N = 3 and K = 5.

| Zero-Based Bit Position | 1-Based Bit Index | Candidate After XOR | Divisible by 5? |
| --- | --- | --- | --- |
| 0 | 1 | 2 | No |
| 1 | 2 | 1 | No |
| 2 | 3 | 7 | No |
| 3 | 4 | 11 | No |
| 4 | 5 | 19 | No |
| 5 | 6 | 35 | Yes |

The algorithm returns 6 immediately after the first successful candidate.

## 12. Time and Space Complexity

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(32), which is O(1) | At most 32 bit positions are checked. |
| Auxiliary space | O(1) | Only a constant number of integer variables are used. |

## 13. Edge Cases and Test Cases

| Case | Input | Expected Output | Purpose |
| --- | --- | --- | --- |
| Example 1 | N = 3, K = 5 | 6 | The sixth bit is the first successful flip |
| Example 2 | N = 3, K = 10 | -1 | No bit position produces a multiple of 10 |
| Smallest positive N | N = 1, K = 2 | 1 | Flipping the rightmost bit produces 0, which is divisible by 2 |
| First successful flip occurs later | N = 5, K = 3 | 5 | Checks that the scan continues past the first four positions |

## 14. Common Mistakes

- Returning a zero-based position instead of the required 1-based bit index.
- Scanning from the leftmost bit when the task asks for the minimum index counted from the right.
- Changing more than one bit in a candidate.
- Forgetting to check all 32 bit positions.
- Using different 32-bit interpretation rules in Java and Python.

## 15. Pattern Identification

**Primary pattern:** Bit Manipulation and Exhaustive Search

**Why this pattern applies:** XOR toggles exactly one chosen bit. The fixed 32-bit width makes a complete scan of all possible bit positions practical.

## 16. Key Takeaways

- XOR with a mask containing one set bit toggles exactly that bit.
- Scan from the rightmost bit to return the minimum 1-based index.
- There are only 32 candidate flips, so the running time is O(1).
- Treat N as a signed 32-bit integer.
