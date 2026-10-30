# Q008: Knight Ranking Parade

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Counting](https://img.shields.io/badge/topic-Counting-blue) ![topic-Nested%20Loops](https://img.shields.io/badge/topic-Nested%20Loops-blue) ![pattern-Array%20Traversal%20with%20Nested%20Counting](https://img.shields.io/badge/pattern-Array%20Traversal%20with%20Nested%20Counting-purple)

## 1. Problem Statement

**Knight Ranking Parade**

In an Arrayland, N knights line up in a row A, each with a unique power level. A knight is considered special if the number of stronger knights to their left is strictly greater than the number of stronger knights to their right. Your task is to find and return an integer value representing the count of special knights in the lineup.

**Note:** A stronger knight is a knight with a higher power level.

## 2. Input Format

Input format:

- Line 1: An integer N, representing the number of knights.
- Line 2: N integer power levels in array A.

Platform parameters: input1 = N and input2 = A.

## 3. Output Format

Return an integer representing the number of special knights in the lineup.

For the standalone programs below, print that integer on one line.

## 4. Constraints

- A contains N integer power levels.
- A stronger knight has a strictly greater power level.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 5
input2: {4, 3, 5, 2, 1}
~~~

**Output**

~~~text
2
~~~

**Explanation**

A knight is special when the number of stronger knights on the left is strictly greater than the number on the right.

- Power 4: zero stronger knights on the left and one (5) on the right; not special.
- Power 3: one (4) on the left and one (5) on the right; not special.
- Power 5: zero stronger knights on either side; not special.
- Power 2: three (4, 3, 5) on the left and zero on the right; special.
- Power 1: four (4, 3, 5, 2) on the left and zero on the right; special.

There are two special knights, so the output is 2.

### Example 2

**Input**

~~~text
input1: 4
input2: {1, 2, 3, 4}
~~~

**Output**

~~~text
0
~~~

**Explanation**

The values are in ascending order. For every position, the number of stronger knights on the left is not greater than the number on the right, so no knight is special.

**Standalone input for the programs below**

~~~text
5
4 3 5 2 1
~~~

## 6. Intuition

For each knight, compare the number of stronger knights before it with the number of stronger knights after it. Count the knight only when the left-side count is strictly greater than the right-side count.

## 7. Brute-Force Approach (When Relevant)

Use two nested loops for each index. Count stronger knights on the left and on the right. The current knight is special when the left count is greater than the right count.

## 8. Optimized Approach

**Core idea:** Evaluate the left and right counts for each position, then apply the special-knight condition.

1. Initialize the answer to zero.
2. For each index i, initialize left and right counts to zero.
3. Scan indices before i and count values strictly greater than A[i].
4. Scan indices after i and count values strictly greater than A[i].
5. If left is greater than right, increment the answer.
6. Return the answer.

- No additional input constraints are specified.

## 9. Java Solution

The Java implementation is a standalone program that reads N and the power levels.

~~~java
import java.util.Scanner;

public class Main
{
    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] a = new int[n];

        for (int i = 0; i < n; i++)
        {
            a[i] = sc.nextInt();
        }

        int count = 0;

        for (int i = 0; i < n; i++)
        {
            int left = 0;
            int right = 0;

            for (int j = 0; j < i; j++)
            {
                if (a[j] > a[i])
                {
                    left++;
                }
            }

            for (int j = i + 1; j < n; j++)
            {
                if (a[j] > a[i])
                {
                    right++;
                }
            }

            if (left > right)
            {
                count++;
            }
        }

        System.out.println(count);
        sc.close();
    }
}
~~~

## 10. Python Solution

The Python implementation is a standalone program that reads N and the power levels.

~~~python
def main():
    n = int(input().strip())
    a = list(map(int, input().split()))

    count = 0

    for i in range(n):
        left = 0
        right = 0

        for j in range(i):
            if a[j] > a[i]:
                left += 1

        for j in range(i + 1, n):
            if a[j] > a[i]:
                right += 1

        if left > right:
            count += 1

    print(count)

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example: A = [4, 3, 5, 2, 1].

| Position (0-Based) | Power | Stronger on Left | Stronger on Right | Special? |
| --- | ---: | ---: | ---: | --- |
| 0 | 4 | 0 | 1 | No |
| 1 | 3 | 1 | 1 | No |
| 2 | 5 | 0 | 0 | No |
| 3 | 2 | 3 | 0 | Yes |
| 4 | 1 | 4 | 0 | Yes |

Final count: 2.

## 12. Time and Space Complexity

Let N be the number of knights.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N^2) | Each position scans the elements to its left and right. |
| Auxiliary space | O(1) | The counting logic uses a fixed number of integer variables, excluding the input array. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| N = 1, A = [5] | 0 | No values exist on either side |
| N = 3, A = [1, 2, 3] | 0 | No position has more greater values to the left |
| N = 4, A = [4, 3, 2, 1] | 3 | Descending-order test |
| N = 3, A = [3, 1, 2] | 1 | Checks the strict left-greater-than-right condition |
| N = 3, A = [2, 1, 3] | 0 | The middle value has an equal number of stronger knights on each side, so the strict comparison fails |

## 14. Common Mistakes

- Counting values equal to the current value; only strictly greater values count.
- Counting the current index on either side.
- Counting a knight when the left count equals the right count; the left count must be greater.
- Swapping the roles of the left and right counts.

## 15. Pattern Identification

**Primary pattern:** Array Traversal with Nested Counting

**Why this pattern applies:** Each element is evaluated using counts from the elements on its left and right. The implementation scans both sides for every index.

## 16. Key Takeaways

- Compare strictly greater values on each side of the current element.
- A knight is counted only when the left count is greater than the right count.
- The implementation takes O(N^2) time.
- A knight is special only when the number of stronger knights on the left is greater than the number on the right.
