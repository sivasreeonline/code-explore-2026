# Q019: Birthday Present

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Greedy](https://img.shields.io/badge/topic-Greedy-blue) ![topic-Cost%20Normalization](https://img.shields.io/badge/topic-Cost%20Normalization-blue) ![topic-Powers%20of%20Two](https://img.shields.io/badge/topic-Powers%20of%20Two-blue) ![pattern-Greedy%20Cost%20Normalization](https://img.shields.io/badge/pattern-Greedy%20Cost%20Normalization-purple)

## 1. Problem Statement

Bruce wants to give his son, Alex, a gift on his birthday which should be very unique. So Bruce purchases a magical gift box from a gift store. The uniqueness of this gift box is that if it gets filled with at least M magical cookies, it can transform into whatever its recipient wishes for.

The same gift store sells N different types of magical cookie packets of different costs given in array costs[]. A packet of type i has 2^(i-1) magical cookies in it and costs cost[i] dollars. The number of packets of each type in the store can be considered infinite.

Calculate the minimum amount of money (in dollars) Bruce has to spend in order to buy at least M magical cookies.

**Note:** Indexing starts from 1.

## 2. Input Format

Parameters:

- input1: An integer array denoting the individual costs of N packets of cookies.
- input2: An integer N denoting the length of the array.
- input3: An integer M.

For standalone testing, line 1 contains N, line 2 contains the N packet costs, and line 3 contains M.

## 3. Output Format

Return the minimum amount (in dollars) Bruce needs to spend in order to buy at least M magical cookies.

## 4. Constraints

- N is the number of packet types and is positive.
- Type i contains 2^(i-1) cookies.
- Packet costs and M are non-negative integers.
- Any number of packets of each type may be purchased.
- Buy at least M cookies.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: {20, 30, 70, 90}
input2: 4
input3: 12
~~~

**Output**

~~~text
150
~~~

**Explanation**

One type-4 packet costs 90 dollars and contains 8 cookies. Two type-2 packets cost 30 dollars each and contain 4 cookies altogether. The total is 12 cookies for 150 dollars, which is the minimum total cost.

### Example 2

**Input**

~~~text
input1: {10000, 1000, 100, 10}
input2: 4
input3: 3
~~~

**Output**

~~~text
10
~~~

**Explanation**

A type-4 packet contains 8 cookies and costs 10 dollars. Since Bruce needs at least 3 cookies, this packet is sufficient and costs less than the other available packet choices.

## 6. Intuition

Packet sizes double at each level. A more expensive packet can sometimes be replaced by two packets of the immediately smaller type, while a larger packet can also be cheaper and satisfy the target on its own. Normalize costs so a type is never treated as more expensive than buying two of the previous size, then consider the cost of covering the remaining requirement or using one extra packet at a level.

## 7. Brute-Force Approach (When Relevant)

A direct dynamic-programming approach could calculate the minimum cost for every cookie count up to M, but that can be expensive when M is large.

## 8. Optimized Approach

**Core idea:** Normalize adjacent packet costs, process packet sizes from largest to smallest, and consider buying one extra packet to cover any remaining requirement.

1. Copy the input costs.
2. For each type after the first, replace its effective cost with the smaller of its listed cost and twice the previous effective cost.
3. Process packet sizes in descending order.
4. Buy as many packets of the current size as fit in the remaining requirement.
5. After each level, consider the accumulated cost plus one additional packet at that level if the requirement is not yet covered.
6. Return the minimum candidate.

The implementation uses capped packet sizes in Java so that powers of two larger than M can still be considered as single overbuying packets without overflowing a machine integer. The same idea is used in Python.

## 9. Java Solution

The standalone program reads the listed parameters and prints the minimum cost. This is a complete standalone program. Costs and the answer use long internally to reduce overflow risk.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Arrays;

public class Main
{
    public static long minimumCost(int[] costs, int n, long m)
    {
        if (m <= 0)
        {
            return 0;
        }
        if (n <= 0 || costs.length < n)
        {
            throw new IllegalArgumentException("N costs must be supplied.");
        }

        long[] effective = new long[n];
        long[] size = new long[n];
        long cap = m + 1;

        size[0] = 1;
        effective[0] = costs[0];

        for (int i = 1; i < n; i++)
        {
            size[i] = size[i - 1] > m / 2 ? cap : size[i - 1] * 2;
            long doubledPrevious = effective[i - 1] > Long.MAX_VALUE / 2
                ? Long.MAX_VALUE : 2 * effective[i - 1];
            effective[i] = Math.min((long) costs[i], doubledPrevious);
        }

        long remaining = m;
        long spent = 0;
        long best = Long.MAX_VALUE;

        for (int i = n - 1; i >= 0; i--)
        {
            long packs = size[i] <= m ? remaining / size[i] : 0;
            spent += packs * effective[i];
            remaining -= packs * size[i];

            long candidate = spent;
            if (remaining > 0)
            {
                candidate += effective[i];
            }
            best = Math.min(best, candidate);
        }

        return best;
    }

    private static int[] readInts(String text)
    {
        String[] tokens = text.trim().split("[^0-9-]+");
        int[] result = new int[tokens.length];
        int count = 0;
        for (String token : tokens)
        {
            if (!token.isEmpty())
            {
                result[count++] = Integer.parseInt(token);
            }
        }
        return Arrays.copyOf(result, count);
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] lines = raw.split("\\R");
        int n = Integer.parseInt(lines[0].trim());
        int[] costs = readInts(lines[1]);
        long m = Long.parseLong(lines[2].trim());

        System.out.println(minimumCost(costs, n, m));
    }
}
~~~

## 10. Python Solution

The standalone program reads the listed parameters and prints the minimum cost. This is a complete standalone program.

~~~python
import re
import sys

def minimum_cost(costs, n, m):
    if m <= 0:
        return 0
    if n <= 0 or len(costs) < n:
        raise ValueError("N costs must be supplied.")

    effective = costs[:n]
    for i in range(1, n):
        effective[i] = min(effective[i], 2 * effective[i - 1])

    sizes = [1] * n
    for i in range(1, n):
        sizes[i] = min(m + 1, sizes[i - 1] * 2)

    remaining = m
    spent = 0
    best = None

    for i in range(n - 1, -1, -1):
        packs = remaining // sizes[i] if sizes[i] <= m else 0
        spent += packs * effective[i]
        remaining -= packs * sizes[i]

        candidate = spent + (effective[i] if remaining > 0 else 0)
        best = candidate if best is None else min(best, candidate)

    return best

def main():
    raw = sys.stdin.read().splitlines()
    if len(raw) < 3:
        return
    n = int(raw[0].strip())
    costs = [int(x) for x in re.findall(r"-?\d+", raw[1])]
    m = int(raw[2].strip())
    print(minimum_cost(costs, n, m))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: costs = [20, 30, 70, 90], N = 4, M = 12.

| Packet type | Cookies per packet | Effective cost | Action | Running cost |
| --- | --- | --- | --- | --- |
| 4 | 8 | 90 | Buy 1 packet; 4 cookies remain | 90 |
| 3 | 4 | 60 | Buy 1 packet; requirement now covered | 150 |
| 2 | 2 | 30 | No further packet is needed for the best exact cover | 150 |

Best cost: 150.

## 12. Time and Space Complexity

Let N be the number of packet types.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N) | Costs are normalized once and packet types are traversed once. |
| Auxiliary space | O(N) | Effective costs and packet sizes are stored. |

## 13. Edge Cases and Test Cases

| Costs | N | M | Expected output | Purpose |
| --- | --- | --- | --- | --- |
| [20, 30, 70, 90] | 4 | 12 | 150 | Calculates the minimum total cost for four packet prices and a target of 12 |
| [10000, 1000, 100, 10] | 4 | 3 | 10 | Checks the minimum-cost choice when packet prices differ by powers of ten |
| [5] | 1 | 1 | 5 | Only one packet type |
| [5, 20] | 2 | 3 | 15 | One type-2 packet represented by two cheaper single-cookie packets, plus one single-cookie packet |
| [9, 10] | 2 | 2 | 10 | One larger packet is cheaper than two smaller packets |
| [7, 8] | 2 | 0 | 0 | No cookies required |

## 14. Common Mistakes

- Treating the packet type number as the number of cookies; type i contains a power-of-two quantity.
- Forgetting that the requirement is at least M, so buying extra cookies is allowed.
- Considering only the exact binary representation of M and missing a cheaper overbuying packet.
- Failing to normalize a packet cost that is more expensive than two packets of the previous type.
- Using 32-bit arithmetic for large accumulated costs without checking the constraints.

## 15. Pattern Identification

**Primary pattern:** Greedy Cost Normalization

**Why this pattern applies:** Packet sizes double, so adjacent cost normalization makes it possible to process the remaining requirement by descending powers of two while checking overbuying candidates.

## 16. Key Takeaways

- Packet type i provides 2^(i-1) cookies.
- The store has an unlimited number of packets of every type.
- Buy at least M cookies, not necessarily exactly M.
- The Java and Python solutions are standalone programs that read the input values and print the minimum cost.
