# Q014: The Dishes

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Sorting](https://img.shields.io/badge/topic-Sorting-blue) ![topic-Custom%20Comparator](https://img.shields.io/badge/topic-Custom%20Comparator-blue) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![pattern-Custom%20Comparator%20Sorting](https://img.shields.io/badge/pattern-Custom%20Comparator%20Sorting-purple)

## 1. Problem Statement

**The Dishes**

In a restaurant, dishes are arranged using a unique sorting technique. After asking the owner, Jack discovered the following sorting criteria:

1. Dishes are first sorted by their starting letters in alphabetical order.
2. If two dishes start with the same letter, they are then sorted by their length, with shorter dishes appearing first.
3. If dishes have the same starting letter and length, they are sorted in standard lexicographical (dictionary) order.

Given an unsorted array of dishes, your task is to implement this sorting technique and return the transformed array.

## 2. Input Format

**Parameters:**

- input1: An integer value, representing the number of dishes.
- input2: A string array representing the dishes.

Platform method signature: `theDishes(cls, input1, input2)`, returning a string array.

## 3. Output Format

Return the transformed array after implementing the above sorting technique.

The platform method returns a string array. In the standalone Java program, the sorted array is printed in bracket notation.

## 4. Constraints

- input1 is the number of dishes.
- input2 contains input1 dish names.
- Each dish name contains at least one character.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 5
input2: {pear, apple, fig, plum, apricot}
~~~

**Output**

~~~text
{apple, apricot, fig, pear, plum}
~~~

**Explanation**

Sort by first letter, then by length, then alphabetically. The `a` dishes come first; `apple` is shorter than `apricot`. `fig` follows. The two `p` dishes have equal length, so `pear` comes before `plum`.

### Example 2

**Input**

~~~text
input1: 3
input2: {bat, bag, bar}
~~~

**Output**

~~~text
{bag, bar, bat}
~~~

**Explanation**

All three words start with `b` and have length 3, so they are ordered alphabetically.

## 6. Intuition

Each dish has three sorting keys: its first character, its total length, and the complete dish name. Sorting by those keys in that order applies all three rules directly.

## 7. Brute-Force Approach (When Relevant)

A simple approach would repeatedly compare pairs of dishes using the required three rules and swap them when they are out of order. A standard sorting routine with a custom comparison key is simpler and more efficient.

## 8. Optimized Approach

**Core idea:** Sort each dish using the tuple (first character, length, full string).

1. Compare the first character of two dish names.
2. If the first characters differ, put the alphabetically smaller first character first.
3. If the first characters match, put the shorter string first.
4. If both starting character and length match, compare the entire strings lexicographically.
5. Return the sorted array.

## 9. Java Solution

The Java solution uses the same sorting rule in a standalone program.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Arrays;

public class Main
{
    public static String[] theDishes(int input1, String[] input2)
    {
        String[] result = Arrays.copyOf(input2, input1);
        Arrays.sort(result, (a, b) ->
        {
            int firstCharComparison = Character.compare(a.charAt(0), b.charAt(0));
            if (firstCharComparison != 0)
            {
                return firstCharComparison;
            }

            int lengthComparison = Integer.compare(a.length(), b.length());
            if (lengthComparison != 0)
            {
                return lengthComparison;
            }

            return a.compareTo(b);
        });

        return result;
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] tokens = raw.split("\\s+");
        int n = Integer.parseInt(tokens[0]);
        if (tokens.length < n + 1)
        {
            throw new IllegalArgumentException("Expected N dish names.");
        }

        String[] dishes = Arrays.copyOfRange(tokens, 1, n + 1);
        System.out.println(Arrays.toString(theDishes(n, dishes)));
    }
}
~~~

## 10. Python Solution

The platform starter signature is preserved exactly, including the class name, method name, parameters, expected return type, and read-only/writable markers.

~~~python
# Read only region start
class UserMainCode(object):
    @classmethod
    def theDishes(cls, input1, input2):
        '''
        input1 : int
        input2 : string[]
        Expected return type : string[]
        '''
        # Read only region end
        # Write code here
        dishes = input2[:input1]
        dishes.sort(key=lambda dish: (dish[0], len(dish), dish))
        return dishes
~~~

## 11. Dry Run

Example 1 input: [pear, apple, fig, plum, apricot].

| Dish | First Character | Length | Position by Sorting Rules |
| --- | --- | --- | --- |
| ackie | a | 5 | Before apple lexicographically |
| apple | a | 5 | After ackie |
| annona | a | 6 | After length-5 dishes |
| apricot | a | 7 | Before avocado lexicographically |
| avocado | a | 7 | After apricot |
| banana | b | 6 | Before blueberry because it is shorter |
| blueberry | b | 9 | Last in this sample |

The transformed array for the dry-run sample is [ackie, apple, annona, apricot, avocado, banana, blueberry].

## 12. Time and Space Complexity

Let N be the number of dishes and L be the maximum dish-name length.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N log N × L) | Sorting performs O(N log N) comparisons, and string comparisons can examine up to L characters. |
| Auxiliary space | O(N) in the standalone implementations | A copy of the array and sorting temporary storage may be used. |

## 13. Edge Cases and Test Cases

| Input Array | Expected Output | Purpose |
| --- | --- | --- |
| [apple, banana, blueberry, avocado, apricot, annona, ackie] | [ackie, apple, annona, apricot, avocado, banana, blueberry] | Checks sorting by first character, then length, then lexicographical order |
| [a, b, ab, acb, aab, baa, bac] | [a, ab, aab, acb, b, baa, bac] | Checks ordering of short words before longer words with matching first characters |
| [a, ab, abc, b, baa, bac, bad] | [a, ab, abc, b, baa, bac, bad] | Verifies that an already sorted list stays in the required order |
| [zoo, ab, a, aa] | [a, aa, ab, zoo] | Starting letter dominates; then length and lexical order |
| [bat, bag, bar] | [bag, bar, bat] | Same first character and length; lexicographical tie-break |
| [apple, apple, ant] | [ant, apple, apple] | Duplicate names are retained |
| [a] | [a] | Single dish |

## 14. Common Mistakes

- Sorting only by the first character and ignoring length.
- Sorting by length before the first character.
- Ignoring lexicographical order when the first character and length are equal.
- Removing duplicate dish names rather than retaining them.
- Using the wrong order of sorting keys.

## 15. Pattern Identification

**Primary pattern:** Custom Comparator Sorting

**Why this pattern applies:** Each item is ordered by multiple keys with an explicit priority: starting character, length, then full string.

## 16. Key Takeaways

- Apply sorting keys in the order stated in the question.
- The full sorting key is (first character, length, full string).
- Preserve duplicate names.
