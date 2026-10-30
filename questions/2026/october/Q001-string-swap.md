# Q001: String Swap

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Simulation](https://img.shields.io/badge/topic-Simulation-blue) ![pattern-Simulation%20and%20String%20Manipulation](https://img.shields.io/badge/pattern-Simulation%20and%20String%20Manipulation-purple)

## 1. Problem Statement

**String Swap**

Given a string S, you need to perform M operations on the string based on an array A of length M, which contains the sequence of operations to be performed. The operations are of two types:

1. Swapping the first and last characters of the string.
2. Swapping the first N/2 and the last N/2 characters of the string, where N is the length of the string.

Your task is to find and return the final string value that will be obtained after performing all M operations in the given sequence.

**Note:**

- The array contains either 1 or 2 as values representing the operation number.
- The length of string S is always even.

## 2. Input Format

**Parameters:**

- input1: A string S, representing the initial string.
- input2: An integer M, representing the size of the array.
- input3: An integer array A, where each element represents the numbers of the operations to be performed.

**Standalone program input convention used by the solutions below:**

- Line 1: String S.
- Line 2: Integer M.
- Line 3: The M operation values, separated by spaces or commas. Braces around the values are also accepted.

For standalone testing, enter S, M, and then the M operation values.

## 3. Output Format

Return the final string value that will be obtained after performing all M operations in the given sequence.

For the standalone programs, print the final string on one line.

## 4. Constraints

- Every operation is either 1 or 2.
- S has even length.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: ABCD
input2: 3
input3: {1,2,1}
~~~

**Output**

~~~text
BADC
~~~

**Explanation**

The operations are applied in order:

1. Operation 1 changes ABCD to DBCA.
2. Operation 2 swaps the first and last halves, changing DBCA to CADB.
3. Operation 1 changes CADB to BADC.

Therefore, BADC is returned.

**Standalone input for the programs below**

~~~text
ABCD
3
1 2 1
~~~

### Example 2

**Input**

~~~text
input1: MERCER
input2: 3
input3: {1,1,2}
~~~

**Output**

~~~text
CERMER
~~~

**Explanation**

1. The first operation changes MERCER to RERCEM.
2. The second operation changes RERCEM back to MERCER.
3. The third operation swaps the two halves, producing CERMER.

**Standalone input for the programs below**

~~~text
MERCER
3
1 1 2
~~~

## 6. Intuition

Each operation changes the current string, and the operations must be performed in the given order. Operation 1 exchanges the two end characters. Operation 2 exchanges the two equal halves of the string. Apply every operation to the current result rather than to the original string.

## 7. Brute-Force Approach (When Relevant)

Process the operations one by one, directly modifying the string after each operation. This is also the natural simulation approach for the problem. Operation 1 takes constant time with a mutable character sequence, while operation 2 rebuilds the string from its two halves.

## 8. Optimized Approach

**Core idea:** Simulate the operations in order, using direct character swaps for operation 1 and a half-string rotation for operation 2.

1. Read S, M, and the operation array A.
2. For operation 1, swap the first and last characters.
3. For operation 2, concatenate the second half followed by the first half.
4. Print the string after all M operations have been applied.

The operation order matters, so the transformations must not be reordered.

## 9. Java Solution

The program accepts the standalone input convention described in Section 3. It also accepts operation values written with commas or braces.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main
{
    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] lines = raw.split("\\R");
        if (lines.length < 2)
        {
            throw new IllegalArgumentException("Expected S, M, and the operation array.");
        }

        String s = lines[0].trim();
        int m = Integer.parseInt(lines[1].trim());
        StringBuilder operationText = new StringBuilder();
        for (int i = 2; i < lines.length; i++)
        {
            operationText.append(lines[i]).append(' ');
        }

        Matcher matcher = Pattern.compile("-?\\d+").matcher(operationText.toString());
        int[] operations = new int[m];
        for (int i = 0; i < m; i++)
        {
            if (!matcher.find())
            {
                throw new IllegalArgumentException("The operation array contains fewer than M values.");
            }
            operations[i] = Integer.parseInt(matcher.group());
        }

        StringBuilder current = new StringBuilder(s);
        int n = current.length();
        if (n % 2 != 0)
        {
            throw new IllegalArgumentException("S must have even length.");
        }

        for (int operation : operations)
        {
            if (operation == 1)
            {
                char first = current.charAt(0);
                char last = current.charAt(n - 1);
                current.setCharAt(0, last);
                current.setCharAt(n - 1, first);
            }
            else if (operation == 2)
            {
                String firstHalf = current.substring(0, n / 2);
                String lastHalf = current.substring(n / 2);
                current = new StringBuilder(lastHalf).append(firstHalf);
            }
            else
            {
                throw new IllegalArgumentException("Each operation must be 1 or 2.");
            }
        }

        System.out.println(current);
    }
}
~~~

## 10. Python Solution

~~~python
import re
import sys

def main():
    lines = sys.stdin.read().strip().splitlines()
    if not lines:
        return
    if len(lines) < 2:
        raise ValueError("Expected S, M, and the operation array.")

    current = lines[0].strip()
    m = int(lines[1].strip())
    operations = [int(value) for value in re.findall(r"-?\d+", " ".join(lines[2:]))]
    if len(operations) < m:
        raise ValueError("The operation array contains fewer than M values.")
    if len(current) % 2 != 0:
        raise ValueError("S must have even length.")

    for operation in operations[:m]:
        if operation == 1:
            current = current[-1] + current[1:-1] + current[0]
        elif operation == 2:
            midpoint = len(current) // 2
            current = current[midpoint:] + current[:midpoint]
        else:
            raise ValueError("Each operation must be 1 or 2.")

    print(current)

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: S = ABCD and A = [1, 2, 1].

| Step | Operation | Action | Result |
| --- | --- | --- | --- |
| Initial | — | Start with the given string | ABCD |
| 1 | 1 | Swap first and last characters | DBCA |
| 2 | 2 | Swap the two halves | CADB |
| 3 | 1 | Swap first and last characters | BADC |

## 12. Time and Space Complexity

Let L be the length of S and M be the number of operations.

| Approach | Time Complexity | Space Complexity | Reason |
| --- | --- | --- | --- |
| Direct simulation | O(M × L) in the worst case | O(L) | Operation 2 rebuilds the string in linear time; it may occur in each operation. |

## 13. Edge Cases and Test Cases

| Case | Standalone Input | Expected Output | Purpose |
| --- | --- | --- | --- |
| Example 1 | S = ABCD, M = 3, A = [1, 2, 1] | BADC | Mixes both operation types |
| Example 2 | S = MERCER, M = 3, A = [1, 1, 2] | CERMER | Applies operation 1 twice before operation 2 |
| Short even string | S = AB, M = 1, A = [1] | BA | Checks swapping the two end characters |
| Half swap | S = ABCD, M = 1, A = [2] | CDAB | Checks operation 2 on its own |

## 14. Common Mistakes

- Applying every operation to the original string rather than the result of the previous operation.
- Swapping only the first two characters with the last two instead of swapping the complete halves for operation 2.
- Forgetting that the string length is guaranteed to be even.
- Treating the operation values as positions rather than operation types.

## 15. Pattern Identification

**Primary pattern:** Simulation and String Manipulation

**Why this pattern applies:** The problem provides an ordered sequence of operations and asks for the final state after all of them are applied.

## 16. Key Takeaways

- Follow the given operation sequence exactly.
- Operation 1 exchanges the end characters; operation 2 exchanges the two equal halves.
- The worst-case time complexity is O(M × L).
