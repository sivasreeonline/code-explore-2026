# Q004: Frequency Sync

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Frequency%20Counting](https://img.shields.io/badge/topic-Frequency%20Counting-blue) ![topic-Hash%20Maps](https://img.shields.io/badge/topic-Hash%20Maps-blue) ![topic-Strings](https://img.shields.io/badge/topic-Strings-blue) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![pattern-Frequency%20Counting%20with%20Hash%20Maps](https://img.shields.io/badge/pattern-Frequency%20Counting%20with%20Hash%20Maps-purple)

## 1. Problem Statement

**Frequency Sync**

You are given a string S of lowercase letters and an integer array A of size N. You have to count how many times the frequency of a character in the string matches the frequency of a number in the array. Your task is to find and return an integer value representing the total count of such matches.

Count one for each pair made of a distinct character in S and a distinct value in A that have the same frequency. N is the length of A; S may have a different length.

## 2. Input Format

**Parameters:**

- input1: An integer value N representing the size of the array.
- input2: An integer array A.
- input3: A string value S.

**Standalone program input convention used by the solutions below:**

- Line 1: Integer N.
- Line 2: The N array values, separated by spaces or commas. Braces around the values are also accepted.
- Line 3: String S.

## 3. Output Format

Return an integer value representing the count of times a character's frequency in the string S is equal to the frequency of a number in the array A.

For the standalone programs, print the total count on one line.

## 4. Constraints

- S contains lowercase English letters.
- A contains N integers.
- N is the length of A; S may have a different length.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 14
input2: {1, 4, 3, 6, 1, 4, 3, 6, 1, 1, 1, 3, 6, 9}
input3: aabcdeeeffggg
~~~

**Output**

~~~text
9
~~~

**Explanation**

The character frequencies in `S` are:

| Character | Frequency in S |
| --- | ---: |
| a | 2 |
| b | 1 |
| c | 1 |
| d | 1 |
| e | 3 |
| f | 2 |
| g | 3 |

The 14 array values have these frequencies:

| Number | Frequency in A |
| --- | ---: |
| 1 | 5 |
| 3 | 3 |
| 4 | 2 |
| 6 | 3 |
| 9 | 1 |

The matching frequency pairs are:

- Characters `a` and `f` each have frequency 2 and match array value `4`, whose frequency is 2: 2 matches.
- Characters `b`, `c`, and `d` each have frequency 1 and match array value `9`, whose frequency is 1: 3 matches.
- Characters `e` and `g` each have frequency 3 and match array values `3` and `6`, both of which occur 3 times: 4 matches.

Total: 2 + 3 + 4 = 9. N is the length of A, and S is processed independently.

**Standalone input for the programs below**

~~~text
14
1,4,3,6,1,4,3,6,1,1,1,3,6,9
aabcdeeeffggg
~~~

### Example 2

**Input**

~~~text
input1: 5
input2: {1,1,1,2,2}
input3: bgbgb
~~~

**Output**

~~~text
2
~~~

**Explanation**

The character frequencies in bgbgb are b = 3 and g = 2.

- The number 1 occurs 3 times in A, so b's frequency matches the frequency of 1.
- The number 2 occurs 2 times in A, so g's frequency matches the frequency of 2.

There are 2 matching pairs.
The frequencies 3 and 2 are not added to the answer; each matching character/value pair contributes one.

**Standalone input for the programs below**

~~~text
5
1,1,1,2,2
bgbgb
~~~

## 6. Intuition

First count how often each distinct number occurs in A. Then count how often each distinct character occurs in S. For each distinct character, add one for every distinct array value with the same occurrence frequency. Do not add the number's occurrence count. If several array values share a frequency, each distinct value contributes a separate character/value pair, as shown in Example 1.

## 7. Brute-Force Approach (When Relevant)

A direct approach can count each character's frequency and, for each distinct character, recount how often every distinct array value occurs. This repeatedly scans the input and does unnecessary work when many characters or array values are present.

## 8. Optimized Approach

**Core idea:** Use frequency maps for the array values and for the characters, plus a map that records how many distinct array values have each occurrence frequency.

1. Count the occurrences of each distinct value in A.
2. Count how many distinct array values have each frequency.
3. Count the occurrences of each distinct character in S.
4. For each distinct character, add the number of distinct array values whose frequency equals that character's frequency.
5. Print the total.

Do not count only whether a frequency exists. If two distinct array values share the same frequency, both contribute a match.

## 9. Java Solution

The program accepts the standalone input convention described in Section 3.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main
{
    private static int[] extractIntegers(String text)
    {
        Matcher matcher = Pattern.compile("-?\\d+").matcher(text);
        ArrayList<Integer> values = new ArrayList<>();
        while (matcher.find())
        {
            values.add(Integer.parseInt(matcher.group()));
        }

        int[] result = new int[values.size()];
        for (int i = 0; i < values.size(); i++)
        {
            result[i] = values.get(i);
        }
        return result;
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(System.in.readAllBytes(), StandardCharsets.UTF_8).trim();
        if (raw.isEmpty())
        {
            return;
        }

        String[] lines = raw.split("\\R");
        if (lines.length < 3)
        {
            throw new IllegalArgumentException("Expected N, the array, and S on separate lines.");
        }

        int n = Integer.parseInt(lines[0].trim());
        int[] values = extractIntegers(lines[1]);
        String s = lines[2].trim();
        if (values.length < n)
        {
            throw new IllegalArgumentException("The array contains fewer than N values.");
        }

        Map<Integer, Integer> valueFrequency = new HashMap<>();
        for (int i = 0; i < n; i++)
        {
            valueFrequency.put(values[i], valueFrequency.getOrDefault(values[i], 0) + 1);
        }

        Map<Integer, Integer> frequencyMultiplicity = new HashMap<>();
        // Count distinct array values at each occurrence frequency.
        for (int frequency : valueFrequency.values())
        {
            frequencyMultiplicity.put(frequency, frequencyMultiplicity.getOrDefault(frequency, 0) + 1);
        }

        Map<Character, Integer> characterFrequency = new HashMap<>();
        for (char ch : s.toCharArray())
        {
            characterFrequency.put(ch, characterFrequency.getOrDefault(ch, 0) + 1);
        }

        int answer = 0;
        // Each addition is one distinct character/number-value pair.
        for (int frequency : characterFrequency.values())
        {
            answer += frequencyMultiplicity.getOrDefault(frequency, 0);
        }

        System.out.println(answer);
    }
}
~~~

## 10. Python Solution

~~~python
import re
import sys
from collections import Counter

def main():
    lines = sys.stdin.read().strip().splitlines()
    if not lines:
        return
    if len(lines) < 3:
        raise ValueError("Expected N, the array, and S on separate lines.")

    n = int(lines[0].strip())
    values = [int(value) for value in re.findall(r"-?\d+", lines[1])]
    s = lines[2].strip()
    if len(values) < n:
        raise ValueError("The array contains fewer than N values.")

    value_frequency = Counter(values[:n])
    frequency_multiplicity = Counter(value_frequency.values())
    character_frequency = Counter(s)
    # Each addition counts one distinct character/number-value pair.
    answer = sum(
        frequency_multiplicity.get(frequency, 0)
        for frequency in character_frequency.values()
    )
    print(answer)

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 2 uses A = [1, 1, 1, 2, 2] and S = bgbgb.

| Item | Frequency | Array value with matching frequency | Added Matches |
| --- | --- | --- | --- |
| Character b | 3 | Number 1 occurs 3 times | 1 |
| Character g | 2 | Number 2 occurs 2 times | 1 |

Final answer: 2.

## 12. Time and Space Complexity

Let N be the size of A and L be the length of S.

| Approach | Time Complexity | Space Complexity | Reason |
| --- | --- | --- | --- |
| Brute force | O(N² + L) with repeated counting | O(1) extra if frequencies are recounted | Repeated scans count array-value and character frequencies. |
| Optimized | O(N + L) average | O(N) | Hash maps count values and characters in linear expected time. |

The optimized solution counts each input element or character a constant number of times on average.

## 13. Edge Cases and Test Cases

| Case | Standalone Input | Expected Output | Purpose |
| --- | --- | --- | --- |
| Example 1 | N = 14, A = [1,4,3,6,1,4,3,6,1,1,1,3,6,9], S = aabcdeeeffggg | 9 | Checks multiple array values sharing a frequency; S has 13 characters |
| Example 2 | N = 5, A = [1,1,1,2,2], S = bgbgb | 2 | Confirms each matching character/value pair counts once, not by array frequency |
| Multiple values with the same frequency | N = 4, A = [1,1,2,2], S = aabb | 4 | Each of two characters matches each of two array values |
| No matches | N = 3, A = [1,2,3], S = aabbcc | 0 | Character frequencies are 2, while each array value occurs once |

## 14. Common Mistakes

- Counting matching array elements rather than distinct array values and their occurrence frequencies.
- Counting each occurrence of a character separately instead of using the frequency of each distinct character.
- Adding the occurrence count of a matching array value to the result instead of adding one for its matching pair.
- Adding only one match when several distinct array values have the same frequency.
- Forgetting that the required result is the total number of matching character-frequency and array-value-frequency pairs.

## 15. Pattern Identification

**Primary pattern:** Frequency Counting with Hash Maps

**Why this pattern applies:** Both the string and the array are grouped by occurrence frequency. A second frequency map lets the solution count how many distinct array values correspond to each character frequency.

## 16. Key Takeaways

- First count occurrences of each distinct array value and each distinct character.
- Different array values with the same frequency contribute separate matches.
- The optimized approach runs in O(N + L) average time.
