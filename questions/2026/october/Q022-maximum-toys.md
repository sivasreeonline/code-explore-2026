# Q022: Maximum Toys

![difficulty-Medium](https://img.shields.io/badge/difficulty-Medium-orange) ![topic-Dynamic%20Programming](https://img.shields.io/badge/topic-Dynamic%20Programming-blue) ![topic-State-Space%20Scheduling](https://img.shields.io/badge/topic-State-Space%20Scheduling-blue) ![topic-Simulation](https://img.shields.io/badge/topic-Simulation-blue) ![pattern-State-Space%20Dynamic%20Programming](https://img.shields.io/badge/pattern-State-Space%20Dynamic%20Programming-purple)

## 1. Problem Statement

David is a toy collector and he wants to buy toys from a street with N toy shops. An array A represents the waiting time between every purchase for each shop but there are some restrictions on buying toys from the shops, which are:

- The waiting time is not applicable for the first purchase.
- David can buy a toy from the shop only if the waiting time has ended after his last purchase from the same shop.
- He can only purchase one toy at a time and purchasing a toy takes him exactly 1 minute.

David has a total of T minutes to spend on buying toys. Your task is to help David find and return an integer value representing the maximum number of toys that he can purchase within the given time constraint.

**Note:**

- Initially he starts at minute 0.
- The array A is using 1-based indexing.

## 2. Input Format

Parameters:

- input1: An integer value N representing the total number of toy shops.
- input2: An integer value T representing the total time.
- input3: An integer array A representing the waiting time to buy toys from each shop.

For standalone testing, line 1 contains N and T; line 2 contains the N waiting times.

## 3. Output Format

Return an integer value representing the maximum number of toys that David can purchase within the given time constraint.

## 4. Constraints

- N and T are non-negative integers.
- A contains N non-negative waiting times.
- Each purchase takes one minute.
- The waiting time applies only after the first purchase from a shop.

## 5. Examples and Explanations

### Example 1

**Input**

~~~text
input1: 4
input2: 7
input3: {2, 3, 5, 4}
~~~

**Output**

~~~text
7
~~~

**Explanation**

One valid purchase schedule is:

- Minute 1: Purchase from shop 4, whose waiting time is 4.
- Minute 2: Purchase from shop 2, whose waiting time is 3.
- Minute 3: Purchase from shop 3, whose waiting time is 5.
- Minute 4: Purchase from shop 1, whose waiting time is 2.
- Minute 5: Purchase from shop 2 again; its waiting time has elapsed.
- Minute 6: Purchase from shop 1 (or shop 4).
- Minute 7: Purchase from shop 4.

Seven purchases are made in seven minutes.

### Example 2

**Input**

~~~text
input1: 2
input2: 5
input3: {5, 6}
~~~

**Output**

~~~text
2
~~~

**Explanation**

The first purchases can be made from the two shops during the first two minutes. Their waiting times are 5 and 6 minutes, so neither shop can be used again within the remaining time. The maximum number of purchases is 2.

## 6. Intuition

A shop's next available minute depends on when David last purchased from that particular shop. A decision made now changes that shop's future availability, so choosing the shop with the shortest waiting time is not always optimal. For example, with waiting times [3, 5] and T = 6, buying from the longer-wait shop first allows four purchases; the shortest-wait-first priority-queue rule makes only three.

The exact approach records the next available minute for every shop. Two schedules that reach the same availability state can be merged, keeping only the one with the larger number of purchases.

## 7. Brute-Force Approach (When Relevant)

A brute-force search can enumerate every possible choice at every minute, including waiting. Different purchase sequences can reach the same next-availability state, so exploring the same state repeatedly wastes work.

## 8. Optimized Approach

**Core idea:** Use state-space dynamic programming over each shop's next available minute.

A state is a vector containing the earliest minute at which each shop can next be used. For each minute, retain the greatest purchase count found for every distinct state.

1. Initially, every shop is available at minute 1 because the waiting restriction does not apply before its first purchase.
2. For each minute from 1 through T, process every state reached so far.
3. Carry forward an idle transition, because waiting can be part of a valid schedule.
4. For each shop whose next available minute is no later than the current minute, create a purchase transition. That shop's next available minute becomes the current minute plus its waiting time.
5. Cap availability values at T + 1 because a shop becoming available after the deadline cannot affect the answer.
6. If multiple schedules reach the same availability vector, keep only the greatest purchase count.
7. After minute T, return the greatest purchase count among the remaining states.

This explores all possible schedules exactly; it does not rely on a shortest-wait-first or longest-wait-first greedy choice.

**Complexity:** The number of states can grow to (T + 1)^N. If S is the maximum number of states at one minute, the algorithm takes O(T × N × S) time and O(N × S) space. This exact dynamic program is best suited to small inputs.

## 9. Java Solution

The standalone program reads N, T, and the waiting times, then prints the maximum purchase count. This is a complete standalone program. It uses immutable availability-state keys so equivalent schedules can be merged safely.

~~~java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Main
{
    private static final class State
    {
        private final long[] readyAt;
        private final int hash;

        State(long[] readyAt)
        {
            this.readyAt = readyAt;
            this.hash = Arrays.hashCode(readyAt);
        }

        @Override
        public boolean equals(Object other)
        {
            if (this == other)
            {
                return true;
            }
            if (!(other instanceof State))
            {
                return false;
            }

            State that = (State) other;
            return Arrays.equals(this.readyAt, that.readyAt);
        }

        @Override
        public int hashCode()
        {
            return hash;
        }
    }

    private static void keepBest(
        Map<State, Integer> states,
        State state,
        int purchases)
    {
        states.merge(state, purchases, Math::max);
    }

    public static int maximumToys(int n, int totalTime, int[] waiting)
    {
        if (n < 0 || totalTime < 0 || waiting.length < n)
        {
            throw new IllegalArgumentException("Invalid N, T, or waiting-time array.");
        }

        for (int i = 0; i < n; i++)
        {
            if (waiting[i] < 0)
            {
                throw new IllegalArgumentException("Waiting times must be non-negative.");
            }
        }

        long[] initialReadyAt = new long[n];
        Arrays.fill(initialReadyAt, 1L);

        Map<State, Integer> states = new HashMap<>();
        states.put(new State(initialReadyAt), 0);

        for (long minute = 1; minute <= totalTime; minute++)
        {
            Map<State, Integer> nextStates = new HashMap<>();

            for (Map.Entry<State, Integer> entry : states.entrySet())
            {
                State state = entry.getKey();
                int purchases = entry.getValue();

                // Waiting is a valid option, even when a shop is available.
                keepBest(nextStates, state, purchases);

                for (int shop = 0; shop < n; shop++)
                {
                    if (state.readyAt[shop] <= minute)
                    {
                        long[] updatedReadyAt = state.readyAt.clone();
                        long nextReady = minute + (long) waiting[shop];
                        updatedReadyAt[shop] = Math.min(
                            (long) totalTime + 1L,
                            nextReady
                        );

                        keepBest(
                            nextStates,
                            new State(updatedReadyAt),
                            purchases + 1
                        );
                    }
                }
            }

            states = nextStates;
        }

        int best = 0;
        for (int purchases : states.values())
        {
            best = Math.max(best, purchases);
        }
        return best;
    }

    public static void main(String[] args) throws Exception
    {
        String raw = new String(
            System.in.readAllBytes(),
            StandardCharsets.UTF_8
        ).trim();

        if (raw.isEmpty())
        {
            return;
        }

        String[] tokens = raw.split("[\\s,{}]+");
        int n = Integer.parseInt(tokens[0]);
        int totalTime = Integer.parseInt(tokens[1]);
        int[] waiting = new int[n];

        for (int i = 0; i < n; i++)
        {
            waiting[i] = Integer.parseInt(tokens[i + 2]);
        }

        System.out.println(maximumToys(n, totalTime, waiting));
    }
}
~~~

## 10. Python Solution

The standalone program reads N, T, and the waiting times, then prints the maximum purchase count. This is a complete standalone program using the same exact availability-state dynamic programming.

~~~python
import re
import sys

def maximum_toys(n, total_time, waiting):
    if n < 0 or total_time < 0 or len(waiting) < n:
        raise ValueError("Invalid N, T, or waiting-time array.")
    if any(value < 0 for value in waiting[:n]):
        raise ValueError("Waiting times must be non-negative.")

    states = {tuple(1 for _ in range(n)): 0}

    for minute in range(1, total_time + 1):
        next_states = {}

        for ready_at, purchases in states.items():
            # Waiting is a valid option, even when a shop is available.
            next_states[ready_at] = max(
                next_states.get(ready_at, -1),
                purchases,
            )

            for shop in range(n):
                if ready_at[shop] <= minute:
                    updated = list(ready_at)
                    updated[shop] = min(
                        total_time + 1,
                        minute + waiting[shop],
                    )
                    key = tuple(updated)
                    next_states[key] = max(
                        next_states.get(key, -1),
                        purchases + 1,
                    )

        states = next_states

    return max(states.values(), default=0)

def main():
    values = [
        int(token)
        for token in re.findall(r"-?\d+", sys.stdin.read())
    ]
    if len(values) < 2:
        return

    n, total_time = values[0], values[1]
    waiting = values[2:2 + n]
    print(maximum_toys(n, total_time, waiting))

if __name__ == "__main__":
    main()
~~~

## 11. Dry Run

Example 1: N = 4, T = 7, A = [2, 3, 5, 4].

The dynamic program considers every feasible purchase schedule and merges schedules that reach the same availability state. The maximum is 7.

| Minute | One valid shop choice | Purchase count |
| --- | --- | --- |
| 1 | Shop 4 | 1 |
| 2 | Shop 2 | 2 |
| 3 | Shop 3 | 3 |
| 4 | Shop 1 | 4 |
| 5 | Shop 2 | 5 |
| 6 | Shop 1 | 6 |
| 7 | Shop 4 | 7 |

Final answer: 7.

## 12. Time and Space Complexity

Let S be the maximum number of distinct availability vectors retained at any one minute. Each vector contains N next-available times, each capped at T + 1, so there can be at most (T + 1)^N distinct vectors.

| Complexity | Result | Reason |
| --- | --- | --- |
| Time | O(T × S × (N + 1)) | For each of T minutes, the algorithm examines each retained state and considers at most N shop-purchase transitions plus one wait transition. |
| Auxiliary space | O((N + 1) × S) | Two adjacent DP layers may each retain up to S availability vectors of length N. |

The worst-case state count is exponential in N, so this exact dynamic program is best suited to small input sizes.

## 13. Edge Cases and Test Cases

| N | T | Waiting times | Expected output | Purpose |
| --- | --- | --- | --- | --- |
| 4 | 7 | [2, 3, 5, 4] | 7 | Checks the maximum number of purchases within a seven-minute time budget |
| 2 | 5 | [5, 6] | 2 | Checks a case where both available purchases fit within the time budget |
| 1 | 5 | [2] | 3 | One shop can be used at minutes 1, 3, and 5 |
| 3 | 3 | [1, 2, 3] | 3 | At most one purchase occurs per minute |
| 0 | 5 | [] | 0 | No shops are available |
| 2 | 0 | [1, 2] | 0 | No purchase time is available |
| 2 | 6 | [3, 5] | 4 | Shows why choosing the shortest waiting time first is not always optimal |
| 2 | 3 | [2, 3] | 3 | Checks a schedule that can make one purchase in each available minute |

## 14. Common Mistakes

- Applying the waiting time before a shop's first purchase.
- Forgetting that only one toy can be purchased in a minute.
- Reusing a shop before its waiting interval has elapsed.
- Always choosing the same shop without considering when it becomes available again.
- Assuming that always choosing the shop with the shortest (or longest) waiting time is optimal.
- Omitting the option to wait when that is needed to align future shop availability.

## 15. Pattern Identification

**Primary pattern:** State-Space Dynamic Programming

**Why this pattern applies:** Each purchase changes one shop's next-availability time. Dynamic programming merges schedules with identical availability vectors while preserving the greatest purchase count for each state.

## 16. Key Takeaways

- The first purchase at each shop has no waiting restriction.
- A purchase takes one minute.
- Each shop becomes available again after its waiting interval.
- The Java and Python solutions are standalone programs that read N, T, and the waiting times.
- The shortest-wait-first priority-queue heuristic is not correct for every input; the state-space DP is exact for the stated timing model, but its worst-case state count is exponential in N.
