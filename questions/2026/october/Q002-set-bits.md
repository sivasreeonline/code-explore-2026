# Q002: Set Bits

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Bit%20Manipulation](https://img.shields.io/badge/topic-Bit%20Manipulation-blue) ![topic-Counting%20Set%20Bits](https://img.shields.io/badge/topic-Counting%20Set%20Bits-blue) ![pattern-Bit%20Manipulation%20and%20Counting](https://img.shields.io/badge/pattern-Bit%20Manipulation%20and%20Counting-purple)

## 1. Problem Statement

**Set Bits**

You are given an integer array of N integers. Your task is to find and return an integer value representing the count of elements in the array where the count of set bits is equal to a given number X.

**Note:** A set bit refers to the value 1 of any bit for a number in its binary representation.

## 2. Input Format

**Parameters:**

- input1: An integer array of N elements.
- input2: An integer value N, representing the length of the array.
- input3: An integer value X, representing the target count of set bits.

**Platform method signature:**

- input1: Integer array.
- input2: Integer N.
- input3: Integer X.

Platform method signature: `UserMainCode.setBits(cls, input1, input2, input3)`. For local testing, the Java standalone runner accepts the array values on line 1, N on line 2, and X on line 3.

## 3. Output Format

Return an integer value representing the count of elements in the array where the count of set bits is equal to a given number X.

For the standalone programs, print that count on one line.

## 4. Constraints

- The array contains N 32-bit integers.
- X is between 0 and 32, inclusive.
- A set bit is a bit whose value is 1.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: {2,3,4,5,7,12}
input2: 6
input3: 2
~~~

**Output**

~~~text
3
~~~

**Explanation**

The binary representations have these counts of 1-bits:

| Number | Binary Representation | Number of Set Bits |
| --- | --- | --- |
| 2 | 0010 | 1 |
| 3 | 0011 | 2 |
| 4 | 0100 | 1 |
| 5 | 0101 | 2 |
| 7 | 0111 | 3 |
| 12 | 1100 | 2 |

The elements with 2 set bits are 3, 5, and 12. Their count is 3.

**Standalone input for the programs below**

~~~text
2,3,4,5,7,12
6
2
~~~

### Example 2

**Input**

~~~text
input1: {9,15,21,27,29,11}
input2: 6
input3: 4
~~~

**Output**

~~~text
3
~~~

**Explanation**

The elements with 4 set bits are 15, 27, and 29. Their count is 3.

**Standalone input for the programs below**

~~~text
9,15,21,27,29,11
6
4
~~~

## 6. Intuition

A set bit is a 1 in the binary representation of a number. Count the set bits in each array element, compare that count with X, and increase the answer when they match.

## 7. Brute-Force Approach (When Relevant)

For every element, repeatedly inspect its binary digits and count the 1 bits. If the count equals X, increase the answer. This direct approach is easy to understand, although a built-in bit-count operation is simpler and avoids writing the counting loop manually.

## 8. Optimized Approach

**Core idea:** Count the set bits of each element with the language's bit-count operation.

1. Initialize the answer to 0.
2. Traverse the first N array elements.
3. Count the set bits in the current element.
4. If that count equals X, increment the answer.
5. Print the final answer.

The Java implementation uses `Integer.bitCount`. The Python implementation masks values to 32 bits, so both versions count bits in the same 32-bit representation, including for negative values.

## 9. Java Solution

The program accepts the standalone input convention described in Section 3.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main
{
    private static int[] extractIntegers(String text)
    {
        Matcher matcher = Pattern.compile("-?\\d+").matcher(text);
        ArrayList<Integer> values = new ArrayList<>();
        while (matcher.find())
        {
            values.add(Integer.parseInt(matcher.group()));
        }

        int[] result = new int[values.size()];
        for (int i = 0; i < values.size(); i++)
        {
            result[i] = values.get(i);
        }
        return result;
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] lines = raw.split("\\R");
        if (lines.length < 3)
        {
            throw new IllegalArgumentException("Expected the array, N, and X on separate lines.");
        }

        int[] values = extractIntegers(lines[0]);
        int n = Integer.parseInt(lines[1].trim());
        int target = Integer.parseInt(lines[2].trim());
        if (values.length < n)
        {
            throw new IllegalArgumentException("The array contains fewer than N values.");
        }

        int answer = 0;
        for (int i = 0; i < n; i++)
        {
            if (Integer.bitCount(values[i]) == target)
            {
                answer++;
            }
        }

        System.out.println(answer);
    }
}
~~~

## 10. Python Solution

Platform signature: the Java method keeps the required class, method, parameters, and return type.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def setBits(cls, input1, input2, input3):
        '''
        input1 : int[]
        input2 : int
        input3 : int
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        answer = 0
        for value in input1[:input2]:
            if (value & 0xFFFFFFFF).bit_count() == input3:
                answer += 1
        return answer
~~~

## 11. Dry Run

Example 1 uses X = 2.

| Number | Set-Bit Count | Equals X = 2? | Running Answer |
| --- | --- | --- | --- |
| 2 | 1 | No | 0 |
| 3 | 2 | Yes | 1 |
| 4 | 1 | No | 1 |
| 5 | 2 | Yes | 2 |
| 7 | 3 | No | 2 |
| 12 | 2 | Yes | 3 |

Final answer: 3.

## 12. Time and Space Complexity

Let N be the number of array elements.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N) | Each element is checked once; counting bits in a 32-bit integer takes constant time. |
| Auxiliary space | O(1) for the counting logic | The loop uses a constant amount of additional working state, excluding the input array and the standalone program's input parsing storage. |

## 13. Edge Cases and Test Cases

| Case | Standalone Input | Expected Output | Purpose |
| --- | --- | --- | --- |
| Example 1 | Array = [2,3,4,5,7,12], N = 6, X = 2 | 3 | Three values have two set bits |
| Example 2 | Array = [9,15,21,27,29,11], N = 6, X = 4 | 3 | Checks a different target count |
| Zero set bits | Array = [0,1,3,7], N = 4, X = 0 | 1 | Checks the value zero |
| No matches | Array = [1,2,3], N = 3, X = 5 | 0 | No element has five set bits |

## 14. Common Mistakes

- Counting the number of digits in a binary representation instead of counting only the 1 bits.
- Comparing the number itself with X instead of comparing its set-bit count with X.
- Returning the matching elements rather than the number of matching elements.
- Counting bits with value 0 instead of bits with value 1.

## 15. Pattern Identification

**Primary pattern:** Bit Manipulation and Counting

**Why this pattern applies:** The condition is based on the number of 1 bits in each integer's binary representation. The problem can be solved by counting set bits independently for every array element.

## 16. Key Takeaways

- A set bit is a bit whose value is 1.
- Count the set bits in each element and compare the count with X.
- Each array element contributes at most one to the answer.
