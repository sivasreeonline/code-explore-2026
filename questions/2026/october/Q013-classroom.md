# Q013: Classroom

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Simulation](https://img.shields.io/badge/topic-Simulation-blue) ![topic-Circular%20Search](https://img.shields.io/badge/topic-Circular%20Search-blue) ![pattern-Simulation%20with%20Circular%20Search](https://img.shields.io/badge/pattern-Simulation%20with%20Circular%20Search-purple)

## 1. Problem Statement

**Classroom**

In a classroom, there are N seats numbered 1 to N, and N students are present. Each student, starting from 1, has a preferred seat represented in an array A. The students enter the classroom one by one and attempt to sit in their preferred seats. If their preferred seat is already occupied, they begin searching for the next available seat in a circular manner.

Your task is to find and return an integer array representing the order in which the seats will get occupied according to their attempts to sit in their preferred seat.

**Rules:**

- Initially, all the seats are empty.
- “Circular” means that if a student's preferred seat is already occupied, they start traversing the seats for the next available seat till the Nth seat and continue the search circularly.

## 2. Input Format

**Parameters:**

- input1: An integer value N, representing the total number of seats present.
- input2: An integer array A, representing preferred seats of students.

For the standalone programs below:

- Line 1: Integer N.
- Line 2: N preferred seat numbers separated by spaces, commas, or enclosed in braces.

## 3. Output Format

Return an integer array representing the order in which the seats will get occupied according to the students' attempts to sit in their preferred seats.

For the standalone programs, print the assigned seat number for each student, in student-arrival order.

## 4. Constraints

- There are N seats and N students.
- Seats are numbered from 1 through N.
- Every preferred seat is between 1 and N.
- Students choose seats in input order; search advances one seat at a time and wraps from N to 1.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 3
input2: {3,2,2}
~~~

**Output**

~~~text
{3,2,1}
~~~

**Explanation**

Initially, all seats are empty.

- The first student prefers seat 3, which is vacant, so seat 3 is occupied. Current arrangement: {3,0,0}.
- The second student prefers seat 2, which is vacant, so seat 2 is occupied. Current arrangement: {3,2,0}.
- The third student prefers seat 2, but it is already occupied. Searching circularly, seat 3 is also occupied, and seat 1 is vacant. The student occupies seat 1.

The assigned seat numbers in student-arrival order are {3,2,1}.

### Example 2

**Input**

~~~text
input1: 2
input2: {2,2}
~~~

**Output**

~~~text
{2,1}
~~~

**Explanation**

- The first student occupies preferred seat 2.
- The second student prefers seat 2, which is occupied. The search wraps around to seat 1, which is vacant.

## 6. Intuition

Process students in their given order. For each student, begin at their preferred seat. If that seat is occupied, move to the next seat, wrapping from N to 1, until an empty seat is found. Record the seat assigned to that student.

## 7. Brute-Force Approach (When Relevant)

The direct simulation is to try seats one at a time for each student, starting at their preferred seat and moving circularly until an empty seat is found. This can scan up to N seats for one student, making the worst-case time complexity O(N²).

## 8. Optimized Approach

**Core idea:** Simulate the circular search directly using an occupied-seat array.

1. Create an occupied array for seats 1 through N, initially marking every seat as empty.
2. Process each preferred seat in student-arrival order.
3. While the current seat is occupied, advance to the next seat.
4. If the next seat number exceeds N, wrap to seat 1.
5. Mark the first empty seat as occupied and record its number in the result array.
6. Return the result array.

Direct simulation is suitable when N is small.

## 9. Java Solution

The standalone Java program reads N and the preferred-seat array, accepting spaces, commas, and braces as separators.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main
{
    private static int[] extractIntegers(String text)
    {
        Matcher matcher = Pattern.compile("-?\\d+").matcher(text);
        ArrayList<Integer> numbers = new ArrayList<>();

        while (matcher.find())
        {
            numbers.add(Integer.parseInt(matcher.group()));
        }

        int[] result = new int[numbers.size()];
        for (int i = 0; i < numbers.size(); i++)
        {
            result[i] = numbers.get(i);
        }
        return result;
    }

    public static int[] assignSeats(int n, int[] preferredSeats)
    {
        if (n < 0 || preferredSeats.length < n)
        {
            throw new IllegalArgumentException("N must be non-negative and N preferred seats must be supplied.");
        }

        boolean[] occupied = new boolean[n + 1];
        int[] assigned = new int[n];

        for (int i = 0; i < n; i++)
        {
            int seat = preferredSeats[i];
            if (seat < 1 || seat > n)
            {
                throw new IllegalArgumentException("Preferred seats must be between 1 and N.");
            }

            while (occupied[seat])
            {
                seat++;
                if (seat > n)
                {
                    seat = 1;
                }
            }

            occupied[seat] = true;
            assigned[i] = seat;
        }

        return assigned;
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        int[] values = extractIntegers(raw);
        if (values.length == 0)
        {
            return;
        }

        int n = values[0];
        if (values.length < n + 1)
        {
            throw new IllegalArgumentException("Expected N preferred seat values.");
        }

        int[] preferredSeats = Arrays.copyOfRange(values, 1, n + 1);
        System.out.println(Arrays.toString(assignSeats(n, preferredSeats)));
    }
}
~~~

## 10. Python Solution

The standalone Java program reads N and the preferred-seat array, accepting spaces, commas, and braces as separators.

~~~python
import re
import sys

def assign_seats(n, preferred_seats):
    if n < 0 or len(preferred_seats) < n:
        raise ValueError("N must be non-negative and N preferred seats must be supplied.")

    occupied = [False] * (n + 1)
    assigned = []

    for preferred in preferred_seats[:n]:
        if preferred < 1 or preferred > n:
            raise ValueError("Preferred seats must be between 1 and N.")

        seat = preferred
        while occupied[seat]:
            seat += 1
            if seat > n:
                seat = 1

        occupied[seat] = True
        assigned.append(seat)

    return assigned

def main():
    values = [int(value) for value in re.findall(r"-?\d+", sys.stdin.read())]
    if not values:
        return

    n = values[0]
    preferred_seats = values[1:n + 1]
    if len(preferred_seats) < n:
        raise ValueError("Expected N preferred seat values.")

    print(assign_seats(n, preferred_seats))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: N = 3, preferred seats = [3, 2, 2].

| Student | Preferred Seat | Search | Assigned Seat | Occupied Seats |
| --- | --- | --- | --- | --- |
| 1 | 3 | Seat 3 is empty | 3 | 3 |
| 2 | 2 | Seat 2 is empty | 2 | 2, 3 |
| 3 | 2 | Seats 2 and 3 are occupied; wrap to seat 1 | 1 | 1, 2, 3 |

Final result in student-arrival order: [3, 2, 1].

## 12. Time and Space Complexity

Let N be the number of students and seats.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N²) worst case | A student may scan many seats before finding an empty seat. |
| Auxiliary space | O(N) | The occupied-seat array and result array each use space proportional to N. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| N = 3, A = [3, 2, 2] | [3, 2, 1] | Tests circular allocation when multiple students prefer the same seat |
| N = 2, A = [2, 2] | [2, 1] | Checks circular fallback when two students request the same seat |
| N = 1, A = [1] | [1] | Single seat and single student |
| N = 4, A = [1, 2, 3, 4] | [1, 2, 3, 4] | Every preferred seat is available |
| N = 4, A = [4, 4, 4, 4] | [4, 1, 2, 3] | Repeated preference with circular wrap-around |

## 14. Common Mistakes

- Forgetting to wrap from seat N back to seat 1.
- Returning each student's preferred seat instead of the seat actually assigned.
- Processing students in a different order from the input array.
- Reusing a seat that has already been occupied.
- Mixing 0-based array indices with 1-based seat numbers.

## 15. Pattern Identification

**Primary pattern:** Simulation with Circular Search

**Why this pattern applies:** Every student follows the same deterministic rule, and the next available seat is found by traversing seats in circular order.

## 16. Key Takeaways

- Seats are assigned in the order students arrive.
- If a preferred seat is occupied, search continues to the next seat with wrap-around.
- The result records assigned seats in student-arrival order.
