# Q025: Energy Consumption

![difficulty-Easy](https://img.shields.io/badge/difficulty-Easy-green) ![topic-Arrays](https://img.shields.io/badge/topic-Arrays-blue) ![topic-Summation](https://img.shields.io/badge/topic-Summation-blue) ![topic-Arithmetic](https://img.shields.io/badge/topic-Arithmetic-blue) ![topic-Unit%20Conversion](https://img.shields.io/badge/topic-Unit%20Conversion-blue) ![topic-String%20Formatting](https://img.shields.io/badge/topic-String%20Formatting-blue) ![pattern-Array%20Aggregation%20and%20Unit%20Conversion](https://img.shields.io/badge/pattern-Array%20Aggregation%20and%20Unit%20Conversion-purple)

## 1. Problem Statement

**Energy Consumption (calculate_consumption)**

In a house, all bulbs are turned on for a fixed T hours each day. You are given an array of N bulb wattages. For M days, you have to calculate the total energy consumption in units (kWh), where 1 unit equals 1000 watt-hours (Wh). The consumption is calculated as follows:

- For each bulb: daily consumption (Wh) = wattage × T
- Over M days: consumption per bulb = wattage × T × M
- Total consumption = sum of the consumption for all bulbs (in Wh)
- Convert watt-hours to kWh by dividing the total by 1000.

Your task is to find and return a string value representing the total energy consumption in kWh with exactly two decimal places.

## 2. Input Format

Parameters:

- input1: An integer value N representing the number of bulbs.
- input2: An integer array representing the wattages of the bulbs.
- input3: An integer value M representing the number of days.
- input4: An integer value T representing the hours each bulb is turned on per day.

For the standalone programs below, the input convention is:

- Line 1: N
- Line 2: N bulb wattages, separated by spaces
- Line 3: M
- Line 4: T

For standalone testing, enter the values in the same order as the parameters.

## 3. Output Format

Return a string representing total energy consumption in kWh, formatted with exactly two digits after the decimal point.

Platform method signature:

~~~java
public String calculate_consumption(int input1, int[] input2, int input3, int input4)
~~~

The method name, underscores, parameter order, and return type are preserved in the solution below.

## 4. Constraints

- N is non-negative, and exactly N wattages are provided.
- Wattages, M, and T are non-negative integers.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
2
60 100
30
5
~~~

**Calculation**

- Total wattage = 60 + 100 = 160 W
- Total operating time = 30 days × 5 hours/day = 150 hours
- Total energy = 160 × 150 = 24,000 Wh
- Convert to kWh: 24,000 / 1,000 = 24.00 kWh

**Expected output**

~~~text
24.00
~~~

### Example 2

**Input**

~~~text
3
9 50 100
10
8
~~~

**Calculation**

- Total wattage = 9 + 50 + 100 = 159 W
- Total operating time = 10 × 8 = 80 hours
- Total energy = 159 × 80 = 12,720 Wh
- Convert to kWh: 12,720 / 1,000 = 12.72 kWh

**Expected output**

~~~text
12.72
~~~

## 6. Intuition

Add the wattages of all bulbs to get the total power used while the bulbs are on. Multiply that total by the number of hours per day and the number of days to calculate watt-hours. Divide by 1,000 to convert watt-hours to kilowatt-hours, then format the result with exactly two decimal places.

## 7. Brute-Force Approach (When Relevant)

A separate brute-force search is not relevant because the task is a direct arithmetic calculation. A straightforward implementation can calculate each bulb's consumption separately and add those values, but summing wattages first gives the same total with less repeated work.

## 8. Optimized Approach

**Core idea:** Sum the bulb wattages once, then multiply by the total operating hours and convert to kWh.

1. Sum the N bulb wattages.
2. Multiply the total wattage by T hours per day and M days.
3. Divide the total watt-hours by 1,000.
4. Return the result formatted with exactly two decimal places.

**Why it works:** Every bulb runs for the same T hours per day over M days. By the distributive property, summing the wattages first and multiplying by T and M is equivalent to adding the energy consumption of each bulb individually.

## 9. Java Solution

Platform method signature: `calculate_consumption(input1, input2, input3, input4)`. The standalone Java driver reads the same values for local testing.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Locale;

// Read only region start
class UserMainCode
{
    public String calculate_consumption(int input1, int[] input2, int input3, int input4)
    {
        // Read only region end
        // Write code here...
        if (input2 == null || input1 <= 0 || input3 <= 0 || input4 <= 0)
        {
            return String.format(Locale.US, "%.2f", 0.0);
        }

        int n = Math.min(input1, input2.length);
        long totalWattage = 0;

        for (int i = 0; i < n; i++)
        {
            totalWattage += input2[i];
        }

        double totalKWh =
            ((double) totalWattage * (double) input4 * (double) input3) / 1000.0;

        return String.format(Locale.US, "%.2f", totalKWh);
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

        String[] tokens = raw.split("\\s+");
        int position = 0;

        int n = Integer.parseInt(tokens[position++]);
        if (n < 0 || tokens.length < n + 3)
        {
            throw new IllegalArgumentException("Expected N, N wattages, M, and T.");
        }

        int[] wattages = new int[n];
        for (int i = 0; i < n; i++)
        {
            wattages[i] = Integer.parseInt(tokens[position++]);
        }

        int m = Integer.parseInt(tokens[position++]);
        int t = Integer.parseInt(tokens[position]);

        String result = new UserMainCode().calculate_consumption(n, wattages, m, t);
        System.out.println(result);
    }
}
~~~

## 10. Python Solution

The Python solution uses the same method name and parameter order. Decimal arithmetic formats the result to two decimal places.

~~~python
from decimal import Decimal, ROUND_HALF_UP
import sys

class UserMainCode(object):
    @classmethod
    def calculate_consumption(cls, input1, input2, input3, input4):
        if input2 is None or input1 <= 0 or input3 <= 0 or input4 <= 0:
            return "0.00"

        n = min(input1, len(input2))
        total_wattage = sum(input2[:n])
        total_wh = total_wattage * input4 * input3
        total_kwh = Decimal(total_wh) / Decimal(1000)

        return format(
            total_kwh.quantize(Decimal("0.01"), rounding=ROUND_HALF_UP),
            ".2f"
        )

def main():
    tokens = list(map(int, sys.stdin.read().split()))
    if not tokens:
        return

    n = tokens[0]
    if n < 0 or len(tokens) < n + 3:
        raise ValueError("Expected N, N wattages, M, and T.")

    wattages = tokens[1:n + 1]
    m = tokens[n + 1]
    t = tokens[n + 2]

    print(UserMainCode.calculate_consumption(n, wattages, m, t))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: N = 2, wattages = [60, 100], M = 30, T = 5.

| Step | Calculation | Result |
| --- | --- | ---: |
| 1 | Sum the wattages: 60 + 100 | 160 W |
| 2 | Calculate total operating hours: 30 × 5 | 150 hours |
| 3 | Calculate watt-hours: 160 × 150 | 24,000 Wh |
| 4 | Convert to kWh: 24,000 / 1,000 | 24.00 kWh |
| 5 | Format to two decimal places | "24.00" |

## 12. Time and Space Complexity

Let N be the number of wattage values considered.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(N) | Each included bulb wattage is visited once. |
| Auxiliary space | O(1) | The summation uses a fixed number of variables, excluding the input array. |

## 13. Edge Cases and Test Cases

| N | Wattages | M | T | Expected output | Purpose |
| ---: | --- | ---: | ---: | --- | --- |
| 2 | [60, 100] | 30 | 5 | 24.00 | Calculates kWh for two bulbs operating for 150 total hours |
| 3 | [9, 50, 100] | 10 | 8 | 12.72 | Checks fractional kWh formatting to exactly two decimal places |
| 1 | [25] | 1 | 5 | 0.13 | Checks rounding of 0.125 kWh to two decimal places |
| 2 | [100, 200] | 1 | 0 | 0.00 | Verifies the zero-output rule when the daily operating time is zero |
| 0 | [] | 30 | 5 | 0.00 | Checks empty-array handling when there are no bulbs |

## 14. Common Mistakes

- Dividing by 1,000 before multiplying by the operating time.
- Forgetting to multiply by both M days and T hours per day.
- Returning a numeric value instead of the required string.
- Printing a variable number of decimal places instead of exactly two.
- Using the number of bulbs N as the wattage sum instead of summing the array values.
- Using a different formula or rounding rule.

## 15. Pattern Identification

**Primary pattern:** Array Aggregation and Unit Conversion

**Why this pattern applies:** The calculation reduces the wattage array to a single sum, multiplies by the common operating duration, and converts watt-hours to kilowatt-hours.

## 16. Key Takeaways

- Total Wh = sum of wattages × hours per day × number of days.
- 1 kWh equals 1,000 Wh.
- Preserve the exact method name calculate_consumption and its parameter order.
- The output is a string with exactly two decimal places.
