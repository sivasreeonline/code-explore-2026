# Q007: Index Sort

> [!NOTE]

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Permutations](https://img.shields.io/badge/topic-Permutations-blue) ![topic-Fixed%20Points](https://img.shields.io/badge/topic-Fixed%20Points-blue) ![topic-Sorting](https://img.shields.io/badge/topic-Sorting-blue) ![pattern-Fixed%20Points%20in%20a%20Permutation](https://img.shields.io/badge/pattern-Fixed%20Points%20in%20a%20Permutation-purple)

## 1. Problem Statement

You are given an array A of size N, which contains a permutation of the numbers from 1 to N. To sort the array in ascending order, you must perform the following operations:

- Choose an integer K from the range [1, N].
- Select two different indices i and j (both not equal to K) and swap the elements at those indices.
- You can perform this swap operation multiple times, but K remains fixed throughout the sorting process.

Your task is to find and return an integer value, representing the count of distinct values of K from 1 to N that allow you to transform the given array into sorted order using the above operations.

Use 1-based indexing.

## 2. Input Format

**Parameters:**

- input1: An integer value N, representing size of the array A.
- input2: An integer array A.

Platform method signature: `public int totalCount(int input1, int[] input2)`. The Python version uses the same parameters and return type.

For the standalone Java test driver below:

- Line 1: N
- Line 2: The N array elements, separated by spaces.

## 3. Output Format

Return an integer value, representing the count of distinct values of K from 1 to N that allow you to transform the given array into sorted order using the above operations.

The platform method returns an integer. The standalone Java driver prints that integer on one line.

## 4. Constraints

- A contains a permutation of the integers from 1 through N.
- Positions are numbered from 1 through N.
- K is chosen from 1 through N.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 6
input2: {2,3,5,4,1,6}
~~~

**Output**

~~~text
2
~~~

**Explanation**

The given array is {2,3,5,4,1,6}.

If K = 4, the following swaps sort the array without using index 4:

1. Swap indices 1 and 5: {1,3,5,4,2,6}.
2. Swap indices 2 and 5: {1,2,5,4,3,6}.
3. Swap indices 3 and 5: {1,2,3,4,5,6}.

K = 4 and K = 6 both allow sorting. The output is 2.

### Example 2

**Input**

~~~text
input1: 4
input2: {1,2,3,4}
~~~

**Output**

~~~text
4
~~~

**Explanation**

The array is already sorted. Since no elements are out of place, every choice of K from 1 through 4 is valid. Therefore, 4 is returned.

## 6. Intuition

The chosen index K cannot be involved in any swap, so the value currently at position K never moves. For the final array to be sorted, that position must already contain K. Every position that already contains its own 1-based index is therefore a valid choice of K.

## 7. Brute-Force Approach (When Relevant)

A direct approach would try every K and simulate whether the other positions can be rearranged without moving position K. This is unnecessary because the value at K is fixed: if A[K] is not K, sorting is impossible for that choice; if A[K] equals K, the remaining values can be sorted using swaps among the other positions.

## 8. Optimized Approach

**Core idea:** Count the fixed points of the permutation, where the value equals its 1-based position.

1. Traverse the array from index 0 to N - 1.
2. For each index i, check whether A[i] equals i + 1.
3. Increment the answer for every match.
4. Return the answer.

**Why it works:** If A[i] is not i + 1, that position cannot be corrected because swaps involving the corresponding K are forbidden. If A[i] equals i + 1, the other elements are exactly the remaining values and can be rearranged into ascending order without touching that position.

## 9. Java Solution

The Java solution preserves the platform method signature. A standalone driver is included for local testing.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

class UserMainCode
{
    // Read only region start
    public int totalCount(int input1, int[] input2)
    {
        // Read only region end
        // Write code here
        int count = 0;
        for (int i = 0; i < input1; i++)
        {
            if (input2[i] == i + 1)
            {
                count++;
            }
        }
        return count;
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

        String[] values = raw.split("\\s+");
        int n = Integer.parseInt(values[0]);
        int[] array = new int[n];
        for (int i = 0; i < n; i++)
        {
            array[i] = Integer.parseInt(values[i + 1]);
        }

        System.out.println(new UserMainCode().totalCount(n, array));
    }
}
~~~

## 10. Python Solution

The platform starter signature is preserved. The original class name, method name, parameter order, expected return type, and writable-region markers are retained.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def totalCount(cls, input1, input2):
        '''
        input1 : int
        input2 : int[]
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        count = 0
        for i in range(input1):
            if input2[i] == i + 1:
                count += 1
        return count
~~~

## 11. Dry Run

Additional example:

Input: N = 4, A = [1, 3, 2, 4]

| 1-Based Position K | A[K] | A[K] Equals K? | Count |
| --- | --- | --- | --- |
| 1 | 1 | Yes | 1 |
| 2 | 3 | No | 1 |
| 3 | 2 | No | 1 |
| 4 | 4 | Yes | 2 |

The answer is 2, because positions 1 and 4 can remain fixed while the elements at positions 2 and 3 are swapped.

## 12. Time and Space Complexity

Let N be the array size.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N) | Every array position is checked once. |
| Auxiliary space | O(1) | The counting method uses only a single counter and loop index, excluding the input array. |

## 13. Edge Cases and Test Cases

| N | Array A | Expected Output | Purpose |
| --- | --- | --- | --- |
| 6 | [2, 3, 5, 4, 1, 6] | 2 | Counts elements already located at their 1-based target positions |
| 4 | [1, 2, 3, 4] | 4 | Counts every element when the array is in ascending identity order |
| 3 | [2, 1, 3] | 1 | Only position 3 is fixed |
| 3 | [3, 1, 2] | 0 | No position is fixed |
| 4 | [1, 3, 2, 4] | 2 | Two fixed positions |
| 1 | [1] | 1 | Smallest valid permutation |

## 14. Common Mistakes

- Comparing A[i] with i instead of i + 1; the positions are 1-based.
- Counting values that appear in the correct position only after sorting, rather than values already at their final positions.
- Assuming a position K can be changed during sorting. The problem forbids swaps involving K.
- Sorting the array first and then counting matching positions; that loses the original permutation information.

## 15. Pattern Identification

**Primary pattern:** Fixed Points in a Permutation

**Why this pattern applies:** Each valid K must already satisfy A[K] = K, because its position cannot change. The answer is the number of positions where the value equals the 1-based index.

## 16. Key Takeaways

- A position excluded from all swaps must already contain its final value.
- For a permutation of 1 through N, a valid fixed position K satisfies A[K] = K.
- Counting those positions solves the problem in O(N) time and O(1) auxiliary space.
- Each valid K must already hold the value K.
