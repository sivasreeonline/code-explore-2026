# Q029: Minimum Shopping Cost

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Arithmetic](https://img.shields.io/badge/topic-Arithmetic-blue) ![topic-Enumeration](https://img.shields.io/badge/topic-Enumeration-blue) ![topic-Cost%20Minimization](https://img.shields.io/badge/topic-Cost%20Minimization-blue) ![pattern-Enumeration%20with%20Cost%20Minimization](https://img.shields.io/badge/pattern-Enumeration%20with%20Cost%20Minimization-purple)

## 1. Problem Statement

Two-item packs cost p2 and four-item packs cost p4. Buy at least target items; extra items are allowed. Return minimum cost.

## 2. Input Format

- p2: Cost of a pack containing 2 items.
- p4: Cost of a pack containing 4 items.
- target: Minimum number of items to buy.

For standalone programs, read p2, p4, and target as three integers.

## 3. Output Format

Return the minimum cost to buy at least target items. Extra items are allowed.

## 4. Constraints

- p2, p4, and target are non-negative integers.
- Buying more than target items is allowed.

## 5. Examples and Explanations

### Example 1

~~~text
p2 = 5
p4 = 8
target = 6
Output = 13
~~~

**Explanation**

One four-item pack costs 8. The remaining two items can be bought as one two-item pack costing 5. Total cost is 8 + 5 = 13. Buying two four-item packs would cost 16, so the cost 13 is lower.

## 6. Intuition

Try different counts of four-item packs. For each count, calculate the remaining number of items. If items remain, round up the number of two-item packs so the purchase covers at least the target. Keep the minimum cost.

## 7. Brute-Force Approach (When Relevant)

Enumerate possible counts of four-item packs from zero up to the number needed to cover the target plus one extra four-pack. For each choice, calculate the required number of two-item packs and the total cost. Keep the minimum.

## 8. Optimized Approach

**Core idea:** Enumerate four-pack counts and complete each purchase with the fewest two-packs needed.

1. Initialize the best cost to a large value.
2. Try each possible number of four-item packs up to the target plus a small overshoot.
3. Calculate remaining = target - 4 × packs4.
4. If remaining is positive, buy the ceiling of remaining divided by 2 two-packs; otherwise, buy no two-packs.
5. Compute packs4 × p4 + packs2 × p2.
6. Return the smallest cost.

The calculation compares each four-pack count with the cheapest two-pack combination for the remaining items. It does not require the four-pack to be cheaper than two two-packs.

## 9. Java Solution

Method signature: `shoppingCart(int p2, int p4, int target)`.

~~~java
import java.util.Scanner;

public class Main
{
    public static int shoppingCart(int p2, int p4, int target)
    {
        if (p2 < 0 || p4 < 0 || target < 0)
        {
            throw new IllegalArgumentException("Costs and target must be non-negative.");
        }

        long best = Long.MAX_VALUE;
        long maxPacks4 = ((long) target + 4) / 4;

        for (long packs4 = 0; packs4 <= maxPacks4; packs4++)
        {
            long remaining = target - packs4 * 4;
            long packs2 = remaining > 0 ? (remaining + 1) / 2 : 0;
            long cost = packs4 * p4 + packs2 * p2;
            best = Math.min(best, cost);
        }

        return Math.toIntExact(best);
    }

    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);
        if (!sc.hasNextInt())
        {
            sc.close();
            return;
        }

        int p2 = sc.nextInt();
        int p4 = sc.nextInt();
        int target = sc.nextInt();
        System.out.println(shoppingCart(p2, p4, target));
        sc.close();
    }
}
~~~

## 10. Python Solution

~~~python
import sys

def shoppingCart(p2, p4, target):
    if p2 < 0 or p4 < 0 or target < 0:
        raise ValueError("Costs and target must be non-negative.")

    best = float("inf")
    max_packs4 = (target + 4) // 4

    for packs4 in range(max_packs4 + 1):
        remaining = target - packs4 * 4
        packs2 = (remaining + 1) // 2 if remaining > 0 else 0
        cost = packs4 * p4 + packs2 * p2
        best = min(best, cost)

    return int(best)

def main():
    values = list(map(int, sys.stdin.read().split()))
    if len(values) < 3:
        return
    print(shoppingCart(values[0], values[1], values[2]))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

For p2 = 5, p4 = 8, target = 6:

| Four-packs | Items covered by four-packs | Remaining items | Two-packs | Total cost |
| --- | --- | --- | --- | --- |
| 0 | 0 | 6 | 3 | 15 |
| 1 | 4 | 2 | 1 | 13 |
| 2 | 8 | -2 | 0 | 16 |

Minimum cost: 13.

## 12. Time and Space Complexity

Let T be the target quantity.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(T) | The number of four-pack counts examined grows linearly with T. |
| Auxiliary space | O(1) | Only a fixed number of counters and cost variables are used. |

## 13. Edge Cases and Test Cases

| p2 | p4 | target | Expected output | Purpose |
| --- | --- | --- | --- | --- |
| 5 | 8 | 6 | 13 | Compares pack combinations and selects one 4-pack plus one 2-pack for the lowest cost |
| 5 | 10 | 10 | 25 | Checks a target that is exactly divisible into two 4-packs |
| 4 | 7 | 1 | 4 | One two-pack is cheaper than overshooting with a four-pack |
| 5 | 3 | 7 | 6 | Two four-packs cost 6 and supply 8 items |
| 3 | 10 | 8 | 6 | Four two-packs are cheaper than two four-packs |
| 5 | 8 | 0 | 0 | No items required |

## 14. Common Mistakes

- Assuming the target must be met exactly when extra items are explicitly allowed.
- Ignoring the possibility that an additional four-pack may be cheaper than multiple two-packs.
- Rounding down the number of two-packs for an odd remainder.
- Enumerating only combinations that do not exceed the target.
- Assuming that the target quantity must be met exactly when extra items are allowed.

## 15. Pattern Identification

**Primary pattern:** Enumeration with Cost Minimization

**Why this pattern applies:** Each possible count of four-item packs determines the minimum number of two-item packs required for that choice. Comparing total costs identifies the minimum.

## 16. Key Takeaways

- The target is a minimum purchase quantity, not necessarily an exact quantity.
- You may buy more than the target quantity.
- Enumerating four-pack counts and filling any deficit with two-packs finds the minimum.
