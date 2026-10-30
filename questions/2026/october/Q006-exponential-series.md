# Q006: Exponential Series

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Geometric%20Progression](https://img.shields.io/badge/topic-Geometric%20Progression-blue) ![topic-Series%20Generation](https://img.shields.io/badge/topic-Series%20Generation-blue) ![topic-Simulation](https://img.shields.io/badge/topic-Simulation-blue) ![pattern-Mathematical%20Series%20Generation%20and%20Simulation](https://img.shields.io/badge/pattern-Mathematical%20Series%20Generation%20and%20Simulation-purple)

## 1. Problem Statement

**Exponential Series**

Tom is a maths teacher who is teaching geometric progression in his class. He starts with a sequence where the first term is A1 and the common ratio is R. Each subsequent term in the sequence is obtained by multiplying the previous term by R. Your task is to find and return an integer array denoting the series till the largest term in the sequence that is less than or equal to an upper limit Z.

## 2. Input Format

**Parameters:**

- input1: An integer value A1 representing the first term of the sequence.
- input2: An integer value R, representing the common ratio of the sequence.
- input3: An integer value Z, representing the upper limit.

Platform method signature: `exponentialSeries(cls, input1, input2, input3)`.

For the standalone Java test driver below:

- Line 1: A1
- Line 2: R
- Line 3: Z

## 3. Output Format

Return an integer array denoting the series till the largest term in the sequence that is less than or equal to a given value Z.

The platform method returns an integer array. The standalone Java driver prints the values in bracket notation.

## 4. Constraints

- A1 is a positive integer.
- R is an integer greater than 1.
- Z is an integer upper limit.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 2
input2: 3
input3: 100
~~~

**Output**

~~~text
(2, 6, 18, 54)
~~~

**Explanation**

The first term is 2, the common ratio is 3, and the upper limit is 100. Repeatedly multiplying each term by 3 produces 2, 6, 18, 54, 162, and so on. The term 162 exceeds Z = 100. Therefore, the series containing terms less than or equal to 100 is (2, 6, 18, 54).

### Example 2

**Input**

~~~text
input1: 1
input2: 5
input3: 1000
~~~

**Output**

~~~text
{1, 5, 25, 125, 625}
~~~

**Explanation**

The sequence is 1, 5, 25, 125, 625. The next term is 3125, which exceeds Z = 1000, so the sequence stops at 625.

## 6. Intuition

Start at the first term A1. Keep including the current term while it does not exceed the upper limit Z. Generate the next term by multiplying the current term by the common ratio R. Stop as soon as the next term exceeds Z.

## 7. Brute-Force Approach (When Relevant)

A separate brute-force search is unnecessary. The sequence itself is generated one term at a time, and every generated term is checked against the upper limit.

## 8. Optimized Approach

**Core idea:** Generate the geometric progression until the next term would exceed Z.

1. Initialize the current term with A1.
2. While the current term is less than or equal to Z, append it to the result.
3. Multiply the current term by R to obtain the next term.
4. Return the collected terms.

The sequence is already in the required order, so no sorting is needed.

## 9. Java Solution

This standalone program includes the solution method and a small input driver. The method returns an integer array, matching the expected platform return type.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Main
{
    public static int[] exponentialSeries(int input1, int input2, int input3)
    {
        if (input1 <= 0 || input2 <= 1)
        {
            throw new IllegalArgumentException(
                "Expected a positive first term and a common ratio greater than 1."
            );
        }

        List<Integer> terms = new ArrayList<>();
        long term = input1;

        while (term <= input3)
        {
            terms.add((int) term);
            term *= input2;
        }

        int[] result = new int[terms.size()];
        for (int i = 0; i < terms.size(); i++)
        {
            result[i] = terms.get(i);
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

        String[] values = raw.split("\\s+");
        int input1 = Integer.parseInt(values[0]);
        int input2 = Integer.parseInt(values[1]);
        int input3 = Integer.parseInt(values[2]);

        System.out.println(Arrays.toString(
            exponentialSeries(input1, input2, input3)
        ));
    }
}
~~~

## 10. Python Solution

The platform starter signature is preserved. Complete the implementation in the writable region; the class name, method name, parameters, and expected return type remain unchanged.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def exponentialSeries(cls, input1, input2, input3):
        '''
        input1 : int
        input2 : int
        input3 : int
        Expected return type : int[]
        '''
        # Read only region end
        # Write code here
        if input1 <= 0 or input2 <= 1:
            raise ValueError(
                "Expected a positive first term and a common ratio greater than 1."
            )

        series = []
        term = input1

        while term <= input3:
            series.append(term)
            term *= input2

        return series
~~~

## 11. Dry Run

For A1 = 2, R = 3, and Z = 100:

| Step | Current Term | Action | Result |
| --- | --- | --- | --- |
| 1 | 2 | 2 is at most 100; append and multiply by 3 | Series: [2], next term: 6 |
| 2 | 6 | 6 is at most 100; append and multiply by 3 | Series: [2, 6], next term: 18 |
| 3 | 18 | 18 is at most 100; append and multiply by 3 | Series: [2, 6, 18], next term: 54 |
| 4 | 54 | 54 is at most 100; append and multiply by 3 | Series: [2, 6, 18, 54], next term: 162 |
| 5 | 162 | 162 exceeds 100; stop | Final series: [2, 6, 18, 54] |

## 12. Time and Space Complexity

Let T be the number of terms included in the returned series.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(T) | Each included term is processed once, followed by one final upper-limit check. |
| Auxiliary space | O(T) | The output array or list stores all T terms. |

## 13. Edge Cases and Test Cases

| A1 | R | Z | Expected Result | Purpose |
| --- | --- | --- | --- | --- |
| 2 | 3 | 100 | [2, 6, 18, 54] | Generates successive powers of the base without exceeding the upper limit |
| 1 | 2 | 1 | [1] | Upper limit equals the first term |
| 1 | 5 | 1000 | [1, 5, 25, 125, 625] | Checks the full progression when the limit allows five terms |
| 3 | 2 | 3 | [3] | Only the first term fits |
| 10 | 2 | 9 | [] | First term already exceeds the limit |

## 14. Common Mistakes

- Including the first term even when it exceeds Z.
- Including the first term that exceeds Z.
- Multiplying by the first term instead of the common ratio.
- Continuing after the current term becomes greater than Z.
- Ignoring the fact that a ratio of 1 or less can make the finite-series interpretation invalid.

## 15. Pattern Identification

**Primary pattern:** Mathematical Series Generation and Simulation

**Why this pattern applies:** Each term is derived directly from the previous term using the common ratio, and generation stops at the specified upper limit.

## 16. Key Takeaways

- A geometric progression multiplies each term by a fixed common ratio.
- Include terms only while they are less than or equal to Z.
- Return the terms in generation order; sorting is unnecessary.
