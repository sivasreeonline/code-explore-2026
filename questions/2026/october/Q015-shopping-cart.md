# Q015: Shopping Cart

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Greedy](https://img.shields.io/badge/topic-Greedy-blue) ![topic-Arithmetic](https://img.shields.io/badge/topic-Arithmetic-blue) ![topic-Cost%20Minimization](https://img.shields.io/badge/topic-Cost%20Minimization-blue) ![pattern-Greedy%20Cost%20Minimization](https://img.shields.io/badge/pattern-Greedy%20Cost%20Minimization-purple)

## 1. Problem Statement

**Shopping Cart**

Tony is visiting a supermarket to buy some coke cans. He realized that there are packs of 2 and 4 which are giving better deals, so he went to that section. A pack of 2 costs X dollars, while a pack of 4 costs Y dollars. You have to suggest the best deals to Tony so that he can buy Z cans of coke. Your task is to find and return an integer value representing the minimum cost that Tony has to pay to buy cans of coke.

**Note:** X < Y <= 2X. This means a 4-pack costs more than one 2-pack but no more than two 2-packs.

## 2. Input Format

**Parameters:**

- input1: An integer value X, representing the cost of a 2-pack.
- input2: An integer value Y, representing the cost of a 4-pack.
- input3: An integer value Z, representing the number of cans Tony has to buy.

Platform method signature: `shoppingCart(cls, input1, input2, input3)`, returning an integer.

## 3. Output Format

Return an integer value representing the minimum cost that Tony has to pay to buy cans of coke.

## 4. Constraints

- X and Y are non-negative pack costs, and Z is a non-negative target quantity.
- X < Y <= 2X.
- Buy at least Z cans at minimum cost.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 5
input2: 10
input3: 10
~~~

**Output**

~~~text
25
~~~

**Explanation**

Tony needs 10 cans. Two packs of 4 and one pack of 2 provide 10 cans and cost 25 dollars. Five packs of 2 also provide 10 cans and cost 25 dollars. The minimum cost is 25.

### Example 2

**Input**

~~~text
input1: 8
input2: 12
input3: 21
~~~

**Output**

~~~text
68
~~~

**Explanation**

Tony needs at least 21 cans. Five packs of 4 provide 20 cans and cost 5 × 12 = 60 dollars. One pack of 2 adds 2 cans and costs 8 dollars. The total is 22 cans for 68 dollars, which satisfies the target at minimum cost.

**Standalone input for the programs below**

~~~text
8
12
21
~~~

## 6. Intuition

A 4-pack supplies twice as many cans as a 2-pack. Compare the cost of larger packs with the cost of the smaller packs needed to cover the same quantity. Because the larger pack price is stated to be no more than two 2-packs, using a 4-pack is never more expensive than replacing it with two 2-packs. Any remaining one or two cans of target quantity can be covered with a 2-pack; for a remainder of three, compare one 4-pack with two 2-packs.

## 7. Brute-Force Approach (When Relevant)

Try each possible number of 4-packs from zero through the number needed to cover Z. For each choice, calculate how many 2-packs are required to cover any remaining cans and keep the minimum cost. This checks several combinations but is unnecessary because pack sizes and the price relationship allow a direct calculation.

## 8. Optimized Approach

**Core idea:** Use as many 4-packs as possible, then choose the cheapest way to cover the remainder. If the target is not divisible by four, compare the remaining 2-pack option with the next 4-pack where needed.

1. Divide Z by 4 to obtain the number of full 4-packs and the remainder.
2. If the remainder is 0, buy only the full 4-packs.
3. If the remainder is 1, add one 2-pack to the full 4-packs, which supplies at least the remaining quantity.
4. If the remainder is 2, add one 2-pack.
5. If the remainder is 3, compare the cost of one additional 4-pack with two 2-packs; choose the cheaper option.
6. Return the minimum total cost.

For Example 1, 10 // 4 = 2 full 4-packs cost 20 dollars, and the remaining 2 cans need one 2-pack costing 5 dollars, for a total of 25. For Example 2, five 4-packs plus one 2-pack provide 22 cans for 68 dollars.

## 9. Java Solution

The Java solution is a standalone program. It uses the same rule: buy at least Z cans at minimum cost.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class Main
{
    public static int shoppingCart(int x, int y, int z)
    {
        if (x < 0 || y < 0 || z < 0)
        {
            throw new IllegalArgumentException("Pack costs and target quantity must be non-negative.");
        }

        long fullPacks = z / 4;
        int remainder = z % 4;
        long cost = fullPacks * y;

        if (remainder == 1)
        {
            cost += x;
        }
        else if (remainder == 2)
        {
            cost += x;
        }
        else if (remainder == 3)
        {
            cost += Math.min((long) y, 2L * x);
        }

        return Math.toIntExact(cost);
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] values = raw.split("\\s+");
        int x = Integer.parseInt(values[0]);
        int y = Integer.parseInt(values[1]);
        int z = Integer.parseInt(values[2]);

        System.out.println(shoppingCart(x, y, z));
    }
}
~~~

## 10. Python Solution

The platform starter signature is preserved exactly, including the class name, method name, parameter order, expected return type, and read-only/writable markers.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def shoppingCart(cls, input1, input2, input3):
        '''
        input1 : int
        input2 : int
        input3 : int
        Expected return type : int
        '''
        # Read only region end
        # Write code here
        x = input1
        y = input2
        z = input3

        if x < 0 or y < 0 or z < 0:
            raise ValueError("Pack costs and target quantity must be non-negative.")

        full_packs, remainder = divmod(z, 4)
        cost = full_packs * y

        if remainder == 1:
            cost += x
        elif remainder == 2:
            cost += x
        elif remainder == 3:
            cost += min(y, 2 * x)

        return cost
~~~

## 11. Dry Run

Example 1: X = 5, Y = 10, Z = 10.

| Step | Calculation | Cost |
| --- | --- | --- |
| 1 | Full 4-packs = 10 // 4 = 2 | 2 × 10 = 20 |
| 2 | Remainder = 10 % 4 = 2 cans | Add one 2-pack: 5 |
| 3 | Total cost | 20 + 5 = 25 |

Final answer: 25.

Example 2: X = 8, Y = 12, Z = 21.

| Step | Calculation | Cost |
| --- | --- | --- |
| 1 | Five 4-packs provide 20 cans | 5 × 12 = 60 |
| 2 | One 2-pack adds 2 cans | 8 |
| 3 | Total | 60 + 8 = 68 |

The purchase provides 22 cans, which is at least 21. Final answer: 68.

## 12. Time and Space Complexity

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(1) | Only a fixed number of arithmetic operations are performed. |
| Auxiliary space | O(1) | A constant number of variables are used. |

## 13. Edge Cases and Test Cases

| X | Y | Z | Expected Output | Purpose |
| --- | --- | --- | --- | --- |
| 5 | 10 | 10 | 25 | Finds the least expensive combination of 2-packs and 4-packs for an exact 10-can target |
| 8 | 12 | 21 | 68 | Checks that buying one extra can-pack is allowed when the target is odd |
| 5 | 9 | 4 | 9 | One 4-pack |
| 5 | 10 | 6 | 15 | One 4-pack and one 2-pack |
| 5 | 10 | 8 | 20 | Two 4-packs |
| 5 | 10 | 7 | 20 | At least 7 cans: two 4-packs supply 8 |
| 5 | 10 | 1 | 5 | At least 1 can: one 2-pack |
| 5 | 10 | 0 | 0 | No cans needed |

## 14. Common Mistakes

- Assuming two 2-packs are always cheaper than one 4-pack.
- Forgetting to account for the remainder after dividing Z by 4.
- Using only full 4-packs when the remainder still needs to be covered.
- Failing to document whether over-purchasing is allowed for odd Z.
- Buying exactly Z cans when buying more can cost less.

## 15. Pattern Identification

**Primary pattern:** Greedy Cost Minimization

**Why this pattern applies:** The problem combines fixed pack sizes with a price relationship that lets the solution prefer larger packs when they are no more expensive than equivalent smaller packs, then handle the remaining quantity.

## 16. Key Takeaways

- A 4-pack costs Y and a 2-pack costs X.
- For Z divisible by 4, the cost is the number of 4-packs multiplied by Y, under the stated price relationship.
- Handle the remainder explicitly when Z is not divisible by 4.
