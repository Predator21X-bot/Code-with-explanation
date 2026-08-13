# 🟦 LeetCode 166 — Fraction to Recurring Decimal

**Difficulty:** Medium
**Pattern:** Hash Map + Long Division
**Time Complexity:** `O(n)` where `n` is the length of the resulting decimal
**Space Complexity:** `O(n)`

The problem asks us to convert a fraction into a string and put the **repeating fractional portion inside parentheses**. For example, `1/2 → "0.5"`, `2/1 → "2"`, and `4/333 → "0.(012)"`. ([LeetCode][1])

---

# 📌 Problem

Given:

```cpp
numerator
denominator
```

return the decimal representation as a string.

If the decimal repeats, put the repeating part inside:

```text
(...)
```

### Examples

```text
1 / 2 = "0.5"

2 / 1 = "2"

4 / 333 = "0.(012)"
```

([LeetCode][1])

---

# 💡 Core Idea

This is basically **long division**.

For example:

```text
4 / 333
```

We know the answer is:

```text
0.012012012...
```

The problem is:

> **How do we detect that `012` is repeating?**

The key observation is:

### A remainder determines what happens next.

If we encounter the **same remainder again**, the exact same division process will repeat.

Therefore:

```text
remainder repeats
        ↓
decimal digits repeat
```

And this is where our **Hash Map** comes in.

---

# ⭐ What do we store in the Hash Map?

We store:

```text
remainder → position in answer string
```

For example:

```text
remainder = 4
position = 2
```

means:

> "When remainder `4` first appeared, we started writing digits at position `2`."

If we encounter remainder `4` again:

```text
same remainder
      ↓
same digits will repeat
      ↓
put '(' at its first position
and ')' at the end
```

---

# Step 1 — Handle the Sign

The numerator and denominator can be negative.

For example:

```text
-1 / 2 = -0.5
```

So first determine whether the answer should be negative:

```cpp
if ((numerator < 0) ^ (denominator < 0))
    ans += "-";
```

`^` is XOR.

It means:

```text
negative / positive → negative
positive / negative → negative
negative / negative → positive
positive / positive → positive
```

---

# ⚠️ Important: Convert to `long long`

The constraints allow values as small as:

```text
-2^31
```

For example:

```text
numerator = -2147483648
```

If we simply do:

```cpp
abs(numerator)
```

with an `int`, we can run into overflow because `2147483648` cannot be represented by a signed 32-bit `int`.

So:

```cpp
long long num = abs((long long)numerator);
long long den = abs((long long)denominator);
```

Now we're safe.

---

# Step 2 — Integer Part

Before dealing with decimals, calculate:

```cpp
num / den
```

For:

```text
4 / 333
```

we get:

```text
4 / 333 = 0
```

So:

```cpp
ans += to_string(num / den);
```

gives:

```text
"0"
```

---

# Step 3 — Find the Remainder

Now:

```cpp
num %= den;
```

For:

```text
4 / 333
```

we have:

```text
4 % 333 = 4
```

So:

```text
remainder = 4
```

If the remainder is `0`, we're done.

For example:

```text
1 / 2
```

Integer part:

```text
0
```

Remainder:

```text
1
```

Not zero, so we need a decimal.

---

# Step 4 — Add Decimal Point

Since there is a fractional part:

```cpp
ans += ".";
```

Now:

```text
"0."
```

---

# Step 5 — Long Division

Now we repeatedly do:

```cpp
remainder *= 10;
digit = remainder / denominator;
remainder %= denominator;
```

This is exactly how we perform long division.

---

# 🧪 Example: `1 / 2`

Start:

```text
num = 1
den = 2
```

Integer part:

```text
1 / 2 = 0
```

So:

```text
ans = "0"
```

Remainder:

```text
1 % 2 = 1
```

Add decimal:

```text
ans = "0."
```

### Iteration

Multiply remainder by 10:

```text
1 × 10 = 10
```

Digit:

```text
10 / 2 = 5
```

Append:

```text
ans = "0.5"
```

New remainder:

```text
10 % 2 = 0
```

Remainder is zero.

Stop.

Answer:

```text
"0.5"
```

---

# 🧪 Now the Interesting Example: `4 / 333`

Start:

```text
num = 4
den = 333
```

Integer part:

```text
4 / 333 = 0
```

So:

```text
ans = "0."
```

Remainder:

```text
4
```

Our map is initially:

```text
{}
```

---

### Iteration 1

Before processing remainder `4`:

```cpp
mp[4] = ans.size();
```

Current answer:

```text
"0."
```

So:

```text
mp[4] = 2
```

Now:

```text
remainder = 4 × 10
          = 40
```

Digit:

```text
40 / 333 = 0
```

Append:

```text
"0.0"
```

New remainder:

```text
40
```

---

### Iteration 2

Remainder:

```text
40
```

Store:

```text
mp[40] = 3
```

Multiply:

```text
40 × 10 = 400
```

Digit:

```text
400 / 333 = 1
```

Append:

```text
"0.01"
```

Remainder:

```text
400 % 333 = 67
```

---

### Iteration 3

Remainder:

```text
67
```

Store:

```text
mp[67] = 4
```

Multiply:

```text
67 × 10 = 670
```

Digit:

```text
670 / 333 = 2
```

Append:

```text
"0.012"
```

Remainder:

```text
670 % 333 = 4
```

🚨 **We have seen remainder `4` before!**

Our map says:

```text
mp[4] = 2
```

And the current answer is:

```text
0.012
  ↑
 position 2
```

So everything starting at position `2` repeats.

That's:

```text
012
```

Therefore:

```text
0.(012)
```

---

# ⭐ Why Does Repeated Remainder Mean Repeated Digits?

This is the key concept of the entire problem.

Suppose we have:

```text
remainder = 4
```

The next operation is always:

```text
4 × 10
```

then:

```text
divide by denominator
```

then get a new remainder.

So if we encounter `4` again:

```text
4 → 40 → digit → new remainder
```

will happen **exactly the same way** as the previous time.

Therefore the same sequence of digits repeats.

### Mental model:

```text
Same remainder
      ↓
Same next calculation
      ↓
Same next digit
      ↓
Same next remainder
      ↓
Same sequence forever
```

That's why the Hash Map is based on **remainders**, not digits.

---

# 🔥 Why Store `ans.size()`?

This line is extremely important:

```cpp
mp[remainder] = ans.size();
```

We're storing:

```text
remainder → where its repeating sequence starts
```

For:

```text
4 / 333
```

when remainder `4` first appears:

```text
ans = "0."
```

Its size is:

```text
2
```

Then we eventually get:

```text
"0.012"
```

and see remainder `4` again.

So:

```cpp
int pos = mp[remainder];
```

gives:

```text
pos = 2
```

Then:

```cpp
ans.insert(pos, "(");
ans += ")";
```

produces:

```text
0.(012)
```

---

# ✅ Complete C++ Solution

```cpp
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {

        // Determine sign
        if ((numerator < 0) ^ (denominator < 0)) {
            // We'll add '-' later
        }

        long long num = abs((long long)numerator);
        long long den = abs((long long)denominator);

        string ans;

        // Add sign
        if ((numerator < 0) ^ (denominator < 0)) {
            ans += "-";
        }

        // Integer part
        ans += to_string(num / den);

        // No fractional part
        long long remainder = num % den;

        if (remainder == 0) {
            return ans;
        }

        // Decimal point
        ans += ".";

        // remainder -> position in ans
        unordered_map<long long, int> mp;

        while (remainder != 0) {

            // Repeating remainder found
            if (mp.count(remainder)) {

                int pos = mp[remainder];

                ans.insert(pos, "(");
                ans += ")";

                return ans;
            }

            // Store where this remainder starts
            mp[remainder] = ans.size();

            // Long division
            remainder *= 10;

            ans += to_string(remainder / den);

            remainder %= den;
        }

        return ans;
    }
};
```

---

# 🧠 Understand This Block Extremely Well

This is the heart of the solution:

```cpp
while (remainder != 0) {

    if (mp.count(remainder)) {
        int pos = mp[remainder];

        ans.insert(pos, "(");
        ans += ")";

        return ans;
    }

    mp[remainder] = ans.size();

    remainder *= 10;

    ans += to_string(remainder / den);

    remainder %= den;
}
```

Think of each iteration as:

```text
1. Have I seen this remainder before?
        ↓
       YES → repeating → add ()
        ↓
       NO
        ↓
2. Remember where this remainder started
        ↓
3. Multiply remainder × 10
        ↓
4. Generate next digit
        ↓
5. Calculate new remainder
```

---

# 🧪 Another Example: `1 / 3`

Start:

```text
num = 1
den = 3
```

Integer:

```text
0
```

Answer:

```text
"0."
```

Remainder:

```text
1
```

### First time remainder `1`

```text
mp[1] = 2
```

Multiply:

```text
1 × 10 = 10
```

Digit:

```text
10 / 3 = 3
```

Answer:

```text
0.3
```

Remainder:

```text
10 % 3 = 1
```

### Remainder `1` again!

```text
mp[1] = 2
```

So:

```text
0.(3)
```

---

# 🧪 Example: `1 / 6`

We get:

```text
0.166666...
```

Process:

```text
remainder = 1
```

Generate:

```text
1
```

Then remainder becomes:

```text
4
```

Generate:

```text
6
```

Remainder becomes:

```text
4
```

We've seen `4` before.

Therefore:

```text
0.1(6)
```

Notice something important:

The **non-repeating portion** is `1`.

The repeating portion is only:

```text
6
```

That's why storing the **position where the remainder first appeared** is so useful.

---

# ⚠️ Important Edge Cases

### 1. Exact integer

```text
2 / 1
```

Answer:

```text
"2"
```

No decimal point is needed.

---

### 2. Finite decimal

```text
1 / 2
```

Answer:

```text
"0.5"
```

Eventually:

```text
remainder = 0
```

So we stop.

---

### 3. Repeating decimal

```text
1 / 3
```

Answer:

```text
"0.(3)"
```

Repeated remainder detects the cycle.

---

### 4. Negative result

```text
-1 / 2
```

Answer:

```text
"-0.5"
```

---

### 5. Both negative

```text
-1 / -2
```

Answer:

```text
"0.5"
```

because:

```text
negative / negative = positive
```

---

# 🧠 Pattern Recognition

This problem is a great example of:

```text
Simulation + Hash Map
```

The Hash Map isn't being used to find a value like Two Sum.

Instead, we're using it to detect a **cycle**.

This pattern appears in many problems:

```text
Current state
     ↓
Have I seen this state before?
     ↓
 YES → cycle detected
 NO  → remember it
```

Here:

```text
state = remainder
```

So the pattern becomes:

```text
remainder
    ↓
seen before?
    ↓
YES → repeating decimal
NO  → store position
```

---

# 🔑 Interview Cheat Sheet

### Sign

```cpp
(numerator < 0) ^ (denominator < 0)
```

### Integer part

```cpp
num / den
```

### Initial remainder

```cpp
remainder = num % den;
```

### Detect repetition

```cpp
if (mp.count(remainder))
```

### Store position

```cpp
mp[remainder] = ans.size();
```

### Long division

```cpp
remainder *= 10;

digit = remainder / den;

remainder %= den;
```

### Add parentheses

```cpp
ans.insert(mp[remainder], "(");
ans += ")";
```

---

# 🎯 The One Sentence to Remember

> **In long division, the remainder determines the next digits; therefore, when the same remainder appears again, the digits from its first position onward must repeat.**

That is the entire trick behind **Fraction to Recurring Decimal**.

[1]: https://leetcode.com/problems/fraction-to-recurring-decimal/description/ "Fraction to Recurring Decimal - LeetCode"
