# Q012: River Current

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Consecutive%20Zero%20Segments](https://img.shields.io/badge/topic-Consecutive%20Zero%20Segments-blue) ![topic-Two-Sided%20Propagation](https://img.shields.io/badge/topic-Two-Sided%20Propagation-blue) ![pattern-Linear%20Array%20Traversal%20and%20Segment%20Processing](https://img.shields.io/badge/pattern-Linear%20Array%20Traversal%20and%20Segment%20Processing-purple)

## 1. Problem Statement

**River Current**

You are given an integer array C of N elements representing water current along a river where:

- Positive values (> 0) represent river current flowing to the right.
- Negative values (< 0) represent river current flowing to the left.
- Zeros (0) represent stagnant sections.

For each stagnant section (0), you have to calculate its net current strength based on adjacent non-zero currents. If both adjacent currents exist, the stronger current (higher absolute magnitude) influences the stagnant section. When two or more zero sections are adjacent, the net current strength for those sections is determined using the nearest non-zero currents on either side.

Your task is to find and return the modified integer array representing the net current strength at each section.

**Important Notes:**

- If a section already has a non-zero current, its value remains unchanged.
- For a 0 section, if only one side has a non-zero current, that current propagates to it.
- If both sides have non-zero currents, the current with the larger absolute value dominates.

When boundary currents have different magnitudes, the larger magnitude fills the zero segment. If the magnitudes are equal, identical currents propagate unchanged and opposite currents cancel to zero.

## 2. Input Format

**Parameters:**

- input1: An integer value N, representing the number of sections.
- input2: An integer array C, representing the river current values.

Platform method signature: `public int[] calculateNetCurrent(int input1, int[] input2)`, returning an integer array.

For standalone testing:

- Line 1: Integer N.
- Line 2: N integer current values separated by spaces.

## 3. Output Format

Return an integer array representing the net current strength at each section.

The standalone programs print the resulting array in bracket notation.

## 4. Constraints

- C contains N signed integers.
- Positive values flow right, negative values flow left, and zero represents a stagnant section.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1 (N): 5
input2 (C): {2, 0, 3, 0, 5}
~~~

**Output**

~~~text
{2, 3, 3, 5, 5}
~~~

**Explanation**

- Index 0: The current is 2 and remains unchanged.
- Index 1: The stagnant section is influenced by 2 on the left and 3 on the right. The stronger current is 3, so this section becomes 3.
- Index 2: The current is 3 and remains unchanged.
- Index 3: The stagnant section is influenced by 3 on the left and 5 on the right. The stronger current is 5, so this section becomes 5.
- Index 4: The current is 5 and remains unchanged.

The modified array is {2, 3, 3, 5, 5}.

### Example 2

**Input**

~~~text
input1 (N): 5
input2 (C): {0, 4, 0, 0, -6}
~~~

**Output**

~~~text
{4, 4, -6, -6, -6}
~~~

**Explanation**

- Index 0: The stagnant section is influenced by 4 on the right.
- Index 1: The current is 4 and remains unchanged.
- Index 2: The stagnant section is influenced by -6 on the right.
- Index 3: The stagnant section is influenced by -6 on the right.
- Index 4: The current is -6 and remains unchanged.

The modified array is {4, 4, -6, -6, -6}.

### Example 3

**Input**

~~~text
input1 (N): 5
input2 (C): {2, 0, -3, 0, 5}
~~~

**Output**

~~~text
{2, -3, -3, 5, 5}
~~~

**Explanation**

- The zero at index 1 is between 2 and -3. The current with the larger absolute magnitude is -3, so the zero becomes -3.
- The zero at index 3 is between -3 and 5. The current with the larger absolute magnitude is 5, so the zero becomes 5.

The result is {2, -3, -3, 5, 5}.

### Example 4

**Input**

~~~text
input1 (N): 3
input2 (C): {4, 0, -4}
~~~

**Output**

~~~text
{4, 0, -4}
~~~

The boundary currents have equal magnitude and opposite directions, so they cancel and the middle section remains stagnant.

## 6. Intuition

Only contiguous groups of zeros need to change. For each group, inspect the nearest non-zero value on either side. If one side is missing, propagate the available current. If both sides exist, the larger absolute magnitude fills the group. Equal values in the same direction propagate unchanged. Equal and opposite currents cancel, leaving the group at zero.

## 7. Brute-Force Approach (When Relevant)

A direct approach could search left and right separately from each zero to find the nearest non-zero values, then decide the result for that zero. In a long run of zeros, those searches repeat the same work. Processing each entire zero segment once avoids repeated scans.

## 8. Optimized Approach

**Core idea:** Process consecutive zero segments using the nearest non-zero values at the segment boundaries.

1. Traverse the array until a zero is found.
2. Record the start of the zero segment and continue to its end.
3. Identify the nearest non-zero value on the left, if one exists, and on the right, if one exists.
4. If only one boundary value exists, fill the zero segment with that value.
5. If both exist, fill the segment with whichever value has the greater absolute magnitude.
6. If the boundary values are equal, propagate their common value. If their magnitudes are equal but their directions oppose, leave the zero segment at 0 under the stated rule.
7. Return the modified array.

## 9. Java Solution

The Java method preserves the platform signature. A standalone driver is included for local testing.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Arrays;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

// Read only region start
class UserMainCode
{
    public int[] calculateNetCurrent(int input1, int[] input2)
    {
        // Read only region end
        // Write code here
        int[] current = Arrays.copyOf(input2, input1);
        int n = current.length;
        int i = 0;

        while (i < n)
        {
            if (current[i] != 0)
            {
                i++;
                continue;
            }

            int start = i;
            while (i < n && current[i] == 0)
            {
                i++;
            }
            int end = i - 1;

            boolean hasLeft = start > 0;
            boolean hasRight = i < n;
            Integer fill = null;

            if (hasLeft && !hasRight)
            {
                fill = current[start - 1];
            }
            else if (!hasLeft && hasRight)
            {
                fill = current[i];
            }
            else if (hasLeft && hasRight)
            {
                int left = current[start - 1];
                int right = current[i];

                if (left == right)
                {
                    fill = left;
                }
                else if (Math.abs((long) left) > Math.abs((long) right))
                {
                    fill = left;
                }
                else if (Math.abs((long) right) > Math.abs((long) left))
                {
                    fill = right;
                }
                // Equal and opposite boundaries cancel under the stated rule.
            }

            if (fill != null)
            {
                for (int j = start; j <= end; j++)
                {
                    current[j] = fill;
                }
            }
        }

        return current;
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

        Matcher matcher = Pattern.compile("-?\\d+").matcher(raw);
        int[] values = new int[raw.length()];
        int count = 0;
        while (matcher.find())
        {
            values[count++] = Integer.parseInt(matcher.group());
        }

        if (count < 1)
        {
            return;
        }

        int n = values[0];
        if (count < n + 1)
        {
            throw new IllegalArgumentException("Expected N current values.");
        }

        int[] input = Arrays.copyOfRange(values, 1, n + 1);
        int[] answer = new UserMainCode().calculateNetCurrent(n, input);
        System.out.println(Arrays.toString(answer));
    }
}
~~~

## 10. Python Solution

The Python solution provides the helper calculate_net_current with the same behavior as the Java method.

~~~python
import re
import sys

def calculate_net_current(input1, input2):
    current = input2[:input1]
    n = len(current)
    i = 0

    while i < n:
        if current[i] != 0:
            i += 1
            continue

        start = i
        while i < n and current[i] == 0:
            i += 1
        end = i - 1

        has_left = start > 0
        has_right = i < n
        fill = None

        if has_left and not has_right:
            fill = current[start - 1]
        elif not has_left and has_right:
            fill = current[i]
        elif has_left and has_right:
            left = current[start - 1]
            right = current[i]
            if left == right:
                fill = left
            elif abs(left) > abs(right):
                fill = left
            elif abs(right) > abs(left):
                fill = right
            # Equal and opposite boundaries cancel under the stated rule.

        if fill is not None:
            for j in range(start, end + 1):
                current[j] = fill

    return current

def main():
    values = [int(value) for value in re.findall(r"-?\d+", sys.stdin.read())]
    if not values:
        return

    n = values[0]
    if len(values) < n + 1:
        raise ValueError("Expected N current values.")

    print(calculate_net_current(n, values[1:n + 1]))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1 uses C = [2, 0, 3, 0, 5]. Example 3 uses C = [2, 0, -3, 0, 5].

| Index | Original Value | Nearest Relevant Currents | Action | Result |
| --- | --- | --- | --- | --- |
| 0 | 2 | — | Non-zero value remains unchanged | 2 |
| 1 | 0 | 2 on left, 3 on right | Stronger magnitude is 3 | 3 |
| 2 | 3 | — | Non-zero value remains unchanged | 3 |
| 3 | 0 | 3 on left, 5 on right | Stronger magnitude is 5 | 5 |
| 4 | 5 | — | Non-zero value remains unchanged | 5 |

Final array: [2, 3, 3, 5, 5].

Example 3: C = [2, 0, -3, 0, 5].

| Index | Original Value | Boundary Currents | Result |
| --- | --- | --- | --- |
| 0 | 2 | — | 2 |
| 1 | 0 | 2 and -3; |-3| is larger | -3 |
| 2 | -3 | — | -3 |
| 3 | 0 | -3 and 5; 5 is larger in absolute magnitude | 5 |
| 4 | 5 | — | 5 |

Final array: [2, -3, -3, 5, 5].

## 12. Time and Space Complexity

Let N be the length of C.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N) | The scan processes each array position and each zero segment once. |
| Auxiliary space | O(1) | The array is modified in place and only a fixed number of variables are used, excluding input storage. |

## 13. Edge Cases and Test Cases

Examples 1–3 demonstrate current propagation. Example 4 demonstrates equal and opposite currents.

| Input Array | Expected Output | Purpose |
| --- | --- | --- |
| [2, 0, 3, 0, 5] | [2, 3, 3, 5, 5] | Fills two isolated zero positions from their stronger neighboring currents |
| [0, 4, 0, 0, -6] | [4, 4, -6, -6, -6] | Handles a leading zero and an internal gap where the negative boundary has greater magnitude |
| [2, 0, 0, -3, 5] | [2, -3, -3, -3, 5] | Tests how a two-zero gap is filled when its boundary currents have opposite signs |
| [2, 0, -3, 0, 5] | [2, -3, -3, 5, 5] | Checks two separate zero gaps with a negative middle current |
| [0, 0, 5] | [5, 5, 5] | Only right boundary exists |
| [-2, 0, 0] | [-2, -2, -2] | Only left boundary exists |
| [0, 0, 0] | [0, 0, 0] | No non-zero boundary currents exist |
| [4, 0, 4] | [4, 4, 4] | Equal boundary values in the same direction propagate unchanged |
| [-4, 0, -4] | [-4, -4, -4] | Equal negative boundary values propagate unchanged |
| [-2147483648, 0, 2147483647] | [-2147483648, -2147483648, 2147483647] | Confirms the minimum signed int has the greater absolute magnitude |
| [2147483647, 0, -2147483648] | [2147483647, -2147483648, -2147483648] | Confirms the minimum signed int dominates when it is on the right |
| [4, 0, -4] | [4, 0, -4] | Equal and opposite currents cancel under the stated rule |

## 14. Common Mistakes

- Changing non-zero values even though they must remain unchanged.
- Considering only immediately adjacent array cells rather than the nearest non-zero boundary values of a zero segment.
- Filling a multi-zero segment inconsistently even though both zeros are between the same boundary currents.
- Comparing signed values instead of absolute magnitudes.
- Leaving a zero between identical boundary currents instead of propagating their shared value.
- Choosing a direction for equal and opposite magnitudes instead of leaving the group at zero.
- Assuming equal and opposite currents flow in either direction instead of canceling to zero.

## 15. Pattern Identification

**Primary pattern:** Linear Array Traversal and Segment Processing

**Why this pattern applies:** Consecutive zeros form segments whose result is determined by the nearest non-zero values at their boundaries.

## 16. Key Takeaways

- Non-zero currents remain unchanged.
- A zero segment takes the value of its only available boundary current when one side is missing.
- When both boundaries exist, the larger absolute magnitude determines the segment value.
- Equal and opposite boundary values cancel to zero.
