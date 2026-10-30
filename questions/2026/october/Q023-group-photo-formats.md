# Q023: Group Photo Formats

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-String%20Construction](https://img.shields.io/badge/topic-String%20Construction-blue) ![topic-Boundary%20Enumeration](https://img.shields.io/badge/topic-Boundary%20Enumeration-blue) ![pattern-String%20Construction%20and%20Boundary%20Enumeration](https://img.shields.io/badge/pattern-String%20Construction%20and%20Boundary%20Enumeration-purple)

## 1. Problem Statement

You are given a string S representing a row of students. A camera captures photos focusing on students between two points marked by parentheses ().

The camera can be placed at any valid boundary position along the row of students—from the left of the very first student (position 0) to the right of the last student (position N).

Your task is to generate and return a single space-separated string containing all valid photo formats obtained by inserting () at every possible position from left to right.

## 2. Input Format

- input1 (S): A string representing the group of students.

For standalone testing, line 1 contains the string S.

## 3. Output Format

Return a space-separated string of all valid photo formats showing where the camera was placed.

## 4. Constraints

- S may be empty.
- Parentheses may be placed at every boundary, including before the first character and after the last.
- Preserve the order of the characters in S.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: abc
~~~

**Output**

~~~text
()abc a()bc ab()c abc()
~~~

**Explanation**

For the string `abc`, the camera can be placed at four boundaries:

1. Before the first character: `()abc`
2. Between `a` and `b`: `a()bc`
3. Between `b` and `c`: `ab()c`
4. After the last character: `abc()`

The formats are returned in left-to-right boundary order.

## 6. Intuition

A string of length N has N + 1 boundaries. Insert the two-character marker `()` at each boundary while keeping the original characters in their original order.

## 7. Brute-Force Approach (When Relevant)

For every boundary from 0 through N, build a new string from the prefix before the boundary, `()`, and the suffix from the boundary onward. This directly follows the statement and is efficient enough for typical interview-sized strings.

## 8. Optimized Approach

**Core idea:** Enumerate all N + 1 insertion positions in order.

1. Initialize an empty list of formats.
2. For each position i from 0 through the length of S, append the prefix S[0:i], `()`, and the suffix S[i:N].
3. Join the formats with a single space.
4. Return the resulting string.

## 9. Java Solution

The recreated statement does not provide the platform starter-code signature. This is a complete standalone program with a helper method.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;

public class Main
{
    public static String groupPhotoFormats(String s)
    {
        List<String> formats = new ArrayList<>();
        for (int i = 0; i <= s.length(); i++)
        {
            formats.add(s.substring(0, i) + "()" + s.substring(i));
        }
        return String.join(" ", formats);
    }

    public static void main(String[] args) throws Exception
    {
        String s = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        System.out.println(groupPhotoFormats(s));
    }
}
~~~

## 10. Python Solution

The recreated statement does not provide the platform starter-code signature. This is a complete standalone function.

~~~python
def group_photo_formats(s):
    formats = []
    for i in range(len(s) + 1):
        formats.append(s[:i] + "()" + s[i:])
    return " ".join(formats)

if __name__ == "__main__":
    print(group_photo_formats(input().rstrip("\n")))
~~~

## 11. Dry Run

For S = `abc`:

| Boundary index | Prefix | Inserted marker | Suffix | Format |
| --- | --- | --- | --- | --- |
| 0 | Empty | `()` | `abc` | `()abc` |
| 1 | `a` | `()` | `bc` | `a()bc` |
| 2 | `ab` | `()` | `c` | `ab()c` |
| 3 | `abc` | `()` | Empty | `abc()` |

Final result: `()abc a()bc ab()c abc()`.

## 12. Time and Space Complexity

Let N be the length of S.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N²) total output construction | There are N + 1 strings, and each has length N + 2. |
| Auxiliary space | O(N²) including the returned output | All N + 1 generated formats are stored before joining. |

## 13. Edge Cases and Test Cases

| Input | Expected Output | Purpose |
| --- | --- | --- |
| `abc` | `()abc a()bc ab()c abc()` | Inserts parentheses at every boundary, including before the first and after the last character |
| `a` | `()a a()` | Single character |
| `ab` | `()ab a()b ab()` | Two boundaries between/around characters |
| `""` | `()` | Empty string has one boundary |
| `race` | `()race r()ace ra()ce rac()e race()` | Preserves order at every boundary |

## 14. Common Mistakes

- Generating only N formats instead of N + 1, which omits the boundary after the last character.
- Mutating the input string after each insertion, which shifts later boundary positions.
- Concatenating the formats without the required separating spaces.

## 15. Pattern Identification

**Primary pattern:** String Construction and Boundary Enumeration

**Why this pattern applies:** Every possible insertion position is enumerated in a fixed left-to-right order, and each output is formed from a prefix and suffix of the original string.

## 16. Key Takeaways

- A string of length N has N + 1 insertion boundaries.
- Insert `()` at each boundary from left to right.
- Preserve the original string and join generated formats with one space.
