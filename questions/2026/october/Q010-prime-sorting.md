# Q010: Prime Sorting

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Sorting](https://img.shields.io/badge/topic-Sorting-blue) ![topic-Digit%20Processing](https://img.shields.io/badge/topic-Digit%20Processing-blue) ![topic-Custom%20Comparator](https://img.shields.io/badge/topic-Custom%20Comparator-blue) ![pattern-Custom%20Comparator%20Sorting](https://img.shields.io/badge/pattern-Custom%20Comparator%20Sorting-purple)

## 1. Problem Statement

**Prime Sorting**

You are a librarian in charge of organizing a collection of N books. Each book has a unique identification number represented by an integer array A.

You want to sort these books based on the frequency of prime digits in their identification numbers.

The sorting criteria are:

1. Sort the numbers in ascending order of the frequency of prime digits.
2. If two numbers have the same frequency of prime digits, sort them in ascending numerical order.

The prime digits are 2, 3, 5, and 7.

Your task is to return the sorted array according to these criteria.

## 2. Input Format

**Parameters:**

- input1: An integer N, representing the number of books.
- input2: An integer array A, containing the identification numbers of the books.

For the standalone programs below:

- Line 1: Integer N.
- Line 2: N integer identification numbers separated by spaces.

## 3. Output Format

Return the sorted array according to the criteria mentioned above.

For the standalone programs, print the resulting array in bracket notation.

## 4. Constraints

- A contains N non-negative integer identification numbers.
- Prime digits are 2, 3, 5, and 7.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
6
123 725 22 45 7 11
~~~

**Output**

~~~text
[11, 7, 45, 22, 123, 725]
~~~

**Explanation**

| Number | Prime Digits | Count |
| --- | --- | --- |
| 123 | 2, 3 | 2 |
| 725 | 7, 2, 5 | 3 |
| 22 | 2, 2 | 2 |
| 45 | 5 | 1 |
| 7 | 7 | 1 |
| 11 | None | 0 |

Sort first by prime-digit count, then by numeric value in ascending order.

## 6. Intuition

Count how many digits in each number belong to the set 2, 3, 5, and 7. This count becomes the primary sorting key. If two numbers have equal counts, compare the numbers themselves and place the smaller one first.

## 7. Brute-Force Approach (When Relevant)

A simple approach is to count the prime digits for every number, then use a sorting method that compares every pair of numbers according to the stated rules. A custom comparator provides the same required ordering without needing to manually implement the entire sorting algorithm.

## 8. Optimized Approach

**Core idea:** Sort using a custom key consisting of the count of prime digits followed by the numeric value.

1. Define a helper that counts digits equal to 2, 3, 5, or 7.
2. Sort the array primarily by that count in ascending order.
3. When two counts are equal, sort the numbers in ascending numerical order.
4. Return the sorted array.

## 9. Java Solution

The Java program reads N and the identification numbers, then prints the sorted array. rather than a platform starter-code signature. This implementation follows its sorting criteria and uses Allman brace style.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Arrays;
import java.util.Scanner;

public class Main
{
    private static int primeCount(int n)
    {
        int count = 0;

        while (n > 0)
        {
            int digit = n % 10;

            if (digit == 2 || digit == 3 || digit == 5 || digit == 7)
            {
                count++;
            }

            n /= 10;
        }

        return count;
    }

    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        Integer[] a = new Integer[n];

        for (int i = 0; i < n; i++)
        {
            a[i] = sc.nextInt();
        }

        Arrays.sort(a, (x, y) ->
        {
            int countX = primeCount(x);
            int countY = primeCount(y);

            if (countX != countY)
            {
                return Integer.compare(countX, countY);
            }

            return Integer.compare(x, y);
        });

        System.out.println(Arrays.toString(a));
        sc.close();
    }
}
~~~

## 10. Python Solution

The Python program reads N and the identification numbers, then prints the sorted array.

~~~python
def prime_count(number):
    count = 0

    while number > 0:
        digit = number % 10
        if digit in (2, 3, 5, 7):
            count += 1
        number //= 10

    return count

def main():
    n = int(input().strip())
    values = list(map(int, input().split()))[:n]

    values.sort(key=lambda number: (prime_count(number), number))
    print(values)

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example input: A = [123, 725, 22, 45, 7, 11].

| Number | Prime-Digit Count | Sorting Group |
| --- | --- | --- |
| 123 | 2 | Count 2 |
| 725 | 3 | Count 3 |
| 22 | 2 | Count 2 |
| 45 | 1 | Count 1 |
| 7 | 1 | Count 1 |
| 11 | 0 | Count 0 |

After sorting by count and then numeric value:

[11, 7, 45, 22, 123, 725]

## 12. Time and Space Complexity

Let N be the number of values and D be the maximum number of digits in a value.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N log N × D) | Sorting makes O(N log N) comparisons, and the comparator counts digits. |
| Auxiliary space | O(N) | Object-array sorting and the sorted result require storage proportional to N; Python sorting also uses temporary storage. |

A cached prime-digit count per value could avoid recalculating counts during comparisons. The implementations use the simpler comparator-based approach.

## 13. Edge Cases and Test Cases

| Input Array | Expected Output | Purpose |
| --- | --- | --- |
| [11, 7, 45, 22, 123, 725] | [11, 7, 45, 22, 123, 725] | Verifies ordering by prime-digit count, then by numeric value |
| [2, 3, 5, 7] | [2, 3, 5, 7] | Every value has one prime digit, so numeric order decides ties |
| [111, 999, 222, 10] | [10, 111, 999, 222] | Checks zero, one, and three prime digits |
| [0, 1, 9] | [0, 1, 9] | Values with no prime digits are ordered numerically |

## 14. Common Mistakes

- Counting all prime numbers rather than counting prime digits within each number.
- Forgetting that repeated prime digits count separately; for example, 22 has two prime digits.
- Sorting numerically before considering prime-digit frequency.
- Using descending order for either sorting criterion.
- Ignoring the numeric tie-break when two values have the same prime-digit count.

## 15. Pattern Identification

**Primary pattern:** Custom Comparator Sorting

**Why this pattern applies:** The ordering depends on a computed property of each value, followed by a secondary numeric comparison.

## 16. Key Takeaways

- The prime digits are 2, 3, 5, and 7.
- Count every occurrence of a prime digit in a number.
- Sort by prime-digit count first and numeric value second.
