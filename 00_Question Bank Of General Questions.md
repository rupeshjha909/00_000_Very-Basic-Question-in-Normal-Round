# General Round Warm-Up Problems — A Practice List

> A practice-ready catalog of the **simple, fast coding questions** asked in general / screening rounds. These reward **speed + clean edge-case handling**, not cleverness. Problems are grouped by the **mini-pattern** each one teaches — learn the pattern once and you unlock a whole family. Each entry gives the task, a one-line **approach hint**, the **edge cases** to call out, and the **LeetCode #** to practice on. Full solutions are intentionally left out so you can *solve* them; a "patterns cheat sheet" and an answer-key of *approaches* are at the end.

> 💡 **How to use this:** pick a pattern section, solve every problem in it back-to-back (they share a technique), time yourself to ~10–15 min each, and always say the edge cases out loud. Tick them off as you go.

---

## Table of Contents
1. [How general rounds are judged](#1-how-general-rounds-are-judged)
2. [The recurring mini-patterns](#2-the-recurring-mini-patterns)
3. [Pattern A — Digit-by-digit with carry](#3-pattern-a--digit-by-digit-with-carry)
4. [Pattern B — XOR tricks (pairs cancel)](#4-pattern-b--xor-tricks-pairs-cancel)
5. [Pattern C — One-pass scan (min / max / running state)](#5-pattern-c--one-pass-scan-min--max--running-state)
6. [Pattern D — Two pointers](#6-pattern-d--two-pointers)
7. [Pattern E — HashMap / frequency count](#7-pattern-e--hashmap--frequency-count)
8. [Pattern F — Bit manipulation](#8-pattern-f--bit-manipulation)
9. [Pattern G — Number theory / math](#9-pattern-g--number-theory--math)
10. [Pattern H — String manipulation](#10-pattern-h--string-manipulation)
11. [Pattern I — Simple stack](#11-pattern-i--simple-stack)
12. [Pattern J — Basic patterns & printing (service-based rounds)](#12-pattern-j--basic-patterns--printing-service-based-rounds)
13. [The Practice Checklist](#13-the-practice-checklist)
14. [Answer Key — approach per problem](#14-answer-key--approach-per-problem)
15. [Patterns Cheat Sheet](#15-patterns-cheat-sheet)

---

## 1. How general rounds are judged

General / screening rounds are a **filter**, not the main event. The interviewer is checking:
- Can you translate a simple spec into **working code quickly**?
- Do you **handle edge cases** unprompted — empty input, negatives, overflow, nulls, single element, duplicates?
- Is your code **clean and readable** (good names, no clutter)?
- Can you **state complexity** and spot the obvious optimization?

So the winning habit is: **restate → name edge cases → code cleanly → test on a small example → state O(time)/O(space).** Do that every time.

---

## 2. The recurring mini-patterns

Almost every warm-up is one of these. Learn the pattern, not the 100 problems:

| Pattern | The trick | Unlocks |
|:--|:--|:--|
| **Carry** | add right-to-left, `carry = sum / 10` | add strings, plus-one, add binary, multiply |
| **XOR** | `x ^ x = 0`, so pairs cancel | single number, missing number, swap |
| **One-pass scan** | track running min/max/sum | max/min, Kadane, stock, majority |
| **Two pointers** | one from each end / fast-slow | reverse, palindrome, move zeros, merge |
| **Frequency map** | count with a HashMap/array | anagram, first-unique, duplicates |
| **Bit tricks** | `n & (n-1)`, shifts | power of two, count bits, subsets |
| **Math/number theory** | Euclid GCD, sieve, digit ops | prime, GCD, reverse int, digit sum |
| **Stack** | push/pop for matching/order | valid parentheses, next greater |

---

## 3. Pattern A — Digit-by-digit with carry

*The core mechanic: walk from the rightmost digit, `sum = da + db + carry`, output `sum % 10`, keep `carry = sum / 10` (always 0 or 1 for two digits).*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| A1 | **Add two numbers as digit arrays/strings; return result and/or the carry** *(your remembered one)* | right-to-left, carry; the leftmost leftover `carry` is the final carry | different lengths; a final carry that adds a new digit (`99+1=100`); leading zeros | 415 / 989 |
| A2 | **Plus One** (add 1 to a digit array) | add 1 with carry from the right; if all 9s, prepend a 1 | `[9,9,9] → [1,0,0,0]` | 66 |
| A3 | **Add Binary** (two binary strings) | same carry loop but base 2: `carry = sum / 2`, digit = `sum % 2` | unequal lengths; final carry | 67 |
| A4 | **Add Two Numbers** (linked lists, digits reversed) | traverse both lists with carry; build result list | lists of different length; trailing carry node | 2 |
| A5 | **Multiply Strings** (harder warm-up) | schoolbook multiply; `num1[i]*num2[j]` lands at positions `i+j, i+j+1` | zeros; result length | 43 |

> 💡 **Verified:** `[9,9] + [1]` → `[1,0,0]`, with a carry generated at two positions. Master A1 and the rest are variations of the same loop.

---

## 4. Pattern B — XOR tricks (pairs cancel)

*`x ^ x = 0` and `x ^ 0 = x`, so XOR-ing everything makes duplicates vanish and leaves the odd one out.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| B1 | **Single Number** (every element twice except one) | XOR all elements → the unique one remains | single-element array | 136 |
| B2 | **Missing Number** (0..n, one missing) | XOR of `0..n` XOR all elements = the missing one | missing is 0 or n | 268 |
| B3 | **Find the two single numbers** (two uniques) | XOR all → split by a set bit → XOR each group | — | 260 |
| B4 | **Swap two numbers without a temp** | `a^=b; b^=a; a^=b;` | a == b (stays correct) | — |

> 💡 **Verified:** `single([4,1,2,1,2]) = 4`; `missing([3,0,1], n=3) = 2`.

---

## 5. Pattern C — One-pass scan (min / max / running state)

*Walk once, keep a tiny bit of running state (best-so-far, current-run, a counter). O(n) time, O(1) space.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| C1 | **Max / Min of an array** | track best in one pass | empty array | — |
| C2 | **Second largest** | track largest + secondLargest; update carefully | duplicates of the max; array of size 1 | — |
| C3 | **Maximum Subarray (Kadane's)** | `cur = max(x, cur+x); best = max(best, cur)` | all negatives (answer is the largest single element) | 53 |
| C4 | **Best Time to Buy/Sell Stock** (one transaction) | track min price so far; best profit = price − minSoFar | prices only decreasing → profit 0 | 121 |
| C5 | **Majority Element** (appears > n/2) | Boyer–Moore: candidate + count; flip candidate when count hits 0 | — | 169 |
| C6 | **Maximum Product Subarray** | track running max AND min (negatives flip) | zeros; negatives | 152 |

> 💡 **Verified:** `kadane([-2,1,-3,4,-1,2,1,-5,4]) = 6`; `majority([2,2,1,1,1,2,2]) = 2`.

---

## 6. Pattern D — Two pointers

*One pointer from each end (or a fast/slow pair). Great for reversing, palindromes, in-place moves, merging.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| D1 | **Reverse a string / array (in place)** | swap `left++` / `right--` until they meet | empty; single char | 344 |
| D2 | **Valid Palindrome** (ignore case/non-alnum) | two pointers inward, skip non-alphanumerics | empty string (valid); all punctuation | 125 |
| D3 | **Move Zeros to the end** (keep order) | slow pointer for next non-zero slot; swap in | all zeros; no zeros | 283 |
| D4 | **Reverse vowels / reverse only letters** | two pointers, swap only when both are the target type | no vowels | 345 / 917 |
| D5 | **Merge two sorted arrays** | compare from the ends (or fronts); fill in order | one array empty; different lengths | 88 |
| D6 | **Remove Duplicates from Sorted Array** (in place) | slow write-pointer; advance on a new value | empty; all same | 26 |
| D7 | **Two Sum II** (sorted input) | pointers from both ends; move by comparing sum to target | no solution | 167 |

---

## 7. Pattern E — HashMap / frequency count

*Count occurrences (HashMap, or an `int[26]`/`int[256]` array), then answer in a second pass.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| E1 | **Valid Anagram** | count chars of A, decrement with B, all zero? (or sort both) | different lengths → false | 242 |
| E2 | **First Unique Character** | frequency map, then first char with count 1 | no unique char → -1 | 387 |
| E3 | **Two Sum** (unsorted) | map value→index; look for `target − x` | no solution; duplicates | 1 |
| E4 | **Contains Duplicate** | add to a set; duplicate if already present | empty | 217 |
| E5 | **Character / element frequency** | build and print the count map | case sensitivity | — |
| E6 | **Intersection of two arrays** | set of A, keep B's elements in it | duplicates (dedupe?) | 349 |
| E7 | **Group words that are rotations/anagrams** | canonical key (sorted string) → bucket | — | 49 |

---

## 8. Pattern F — Bit manipulation

*`n & (n-1)` clears the lowest set bit; `n & 1` tests the last bit; `<<`/`>>` shift. Handy, O(1)-ish tricks.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| F1 | **Power of Two** | `n > 0 && (n & (n-1)) == 0` | 0 and negatives → false | 231 |
| F2 | **Number of 1 Bits (Hamming weight)** | Brian Kernighan: `n &= n-1` until 0, count | 0 → 0; negative (treat as unsigned) | 191 |
| F3 | **Count bits for 0..n** | `dp[i] = dp[i>>1] + (i&1)` | n = 0 | 338 |
| F4 | **Reverse bits of a 32-bit int** | shift result left, OR in `n & 1`, shift n right, 32× | — | 190 |
| F5 | **Power of Four** | power of two AND the set bit is in an even position | — | 342 |

> 💡 **Verified:** `pow2(16)=true, pow2(18)=false`; `setbits(13)=3`.

---

## 9. Pattern G — Number theory / math

*Digit operations, Euclid's GCD, primality, factorial/Fibonacci. Watch for overflow.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| G1 | **Reverse Integer** | pop last digit (`x%10`), push into result (`r*10+d`); **check 32-bit overflow** | negatives; trailing zeros (`120→21`); overflow → return 0 | 7 |
| G2 | **Palindrome Number** (no string) | reverse half the number and compare | negatives → false; trailing 0 (except 0 itself) | 9 |
| G3 | **Sum of digits / digital root** | loop `% 10` and `/ 10`; digital root = `1 + (n-1)%9` | 0 | 258 |
| G4 | **GCD (Euclid) / LCM** | `gcd(a,b)=gcd(b, a%b)`; `lcm=a/gcd*b` | b = 0; overflow in lcm | — |
| G5 | **Prime check / Sieve of Eratosthenes** | trial-divide up to √n; sieve for ranges | n < 2; even numbers | 204 |
| G6 | **Factorial / Fibonacci** (iterative + recursive) | iterative avoids stack overflow; watch overflow (use long/BigInteger) | n = 0; large n | 509 |
| G7 | **FizzBuzz** | `%3`, `%5`, both → "FizzBuzz" | order of checks (both first) | 412 |
| G8 | **Armstrong / perfect number** | sum of digit powers / sum of divisors | 1-digit numbers | — |
| G9 | **Decimal ↔ binary conversion** | repeated `%2` / `/2`, or built-ins | 0; negatives | — |
| G10 | **Count trailing zeros in n!** | count factors of 5: `n/5 + n/25 + …` | n < 5 → 0 | 172 |

> 💡 **Verified:** `reverseInt(1534236469)` → 0 (overflow); `reverseInt(-120)` → -21; `gcd(48,18)=6`.

---

## 10. Pattern H — String manipulation

*Scan, build, or transform strings. Mind immutability (use StringBuilder in Java) and edge cases.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| H1 | **Reverse words in a sentence** | split on spaces, reverse order, join (trim extra spaces) | leading/trailing/multiple spaces | 151 |
| H2 | **Longest Common Prefix** | compare char-by-char across all strings, stop at first mismatch | empty array; no common prefix | 14 |
| H3 | **String Compression** ("aaabb"→"a3b2") | count runs; only replace if shorter | run length 1 (no number); run length ≥ 10 | 443 |
| H4 | **Count vowels / consonants** | single pass, check membership | case; non-letters | — |
| H5 | **Remove duplicate characters** | keep a seen-set, build result | preserve order? | — |
| H6 | **Check two strings are rotations** | `(s1+s1).contains(s2)` and equal lengths | different lengths → false | — |
| H7 | **Capitalize / title case / toggle case** | per-char transform | multiple spaces | — |
| H8 | **Implement `indexOf` / `strStr`** (substring search) | sliding compare (or KMP for bonus) | empty needle → 0; needle longer than haystack | 28 |
| H9 | **Count and Say** | describe the previous term run-by-run | n = 1 | 38 |

---

## 11. Pattern I — Simple stack

*Push/pop to track matching or "most recent" — parentheses, next-greater, etc.*

| # | Problem | Approach hint | Edge cases | LC |
|:-:|:--|:--|:--|:--|
| I1 | **Valid Parentheses** (`()[]{}`) | push expected closer on open; on close, pop & compare | empty (valid); unmatched at end | 20 |
| I2 | **Min Stack** (push/pop/top/getMin in O(1)) | keep a parallel min-stack (or store (val, minSoFar)) | getMin after pops | 155 |
| I3 | **Next Greater Element** | monotonic decreasing stack of indices | no greater → -1 | 496 |
| I4 | **Baseball Game / evaluate simple ops** | stack of scores; apply ops | invalid ops | 682 |
| I5 | **Remove adjacent duplicates** | push char; if equals top, pop instead | "aaaa"; empty result | 1047 |

---

## 12. Pattern J — Basic patterns & printing (service-based rounds)

*Common in service-company/campus rounds — pure loops & indexing.*

| # | Problem | Approach hint |
|:-:|:--|:--|
| J1 | **Print a pyramid / right triangle / inverted triangle** | nested loops; spaces then stars |
| J2 | **Pascal's Triangle (a row or full)** | each entry = sum of the two above (`C(n,k)`) — LC 118 |
| J3 | **Floyd's triangle / number pyramids** | running counter in nested loops |
| J4 | **Diamond / hollow patterns** | combine upper + mirrored lower; print border only for hollow |
| J5 | **Spiral print of a matrix** | four boundaries (top/bottom/left/right), shrink inward — LC 54 |
| J6 | **Rotate a matrix 90°** | transpose then reverse each row — LC 48 |
| J7 | **Multiplication table / star patterns** | nested loops |

---

## 13. The Practice Checklist

Tick these off as you solve them (aim: clean + edge-cases + complexity, ≤15 min each).

```
CARRY        [ ] A1 add digits/carry   [ ] A2 plus one    [ ] A3 add binary   [ ] A4 add two numbers   [ ] A5 multiply strings
XOR          [ ] B1 single number      [ ] B2 missing     [ ] B3 two singles  [ ] B4 swap no temp
ONE-PASS     [ ] C1 max/min  [ ] C2 2nd largest  [ ] C3 Kadane  [ ] C4 stock  [ ] C5 majority  [ ] C6 max product
TWO POINTER  [ ] D1 reverse  [ ] D2 palindrome  [ ] D3 move zeros  [ ] D4 reverse vowels  [ ] D5 merge  [ ] D6 dedupe  [ ] D7 two-sum II
FREQ MAP     [ ] E1 anagram  [ ] E2 first unique  [ ] E3 two sum  [ ] E4 contains dup  [ ] E5 freq  [ ] E6 intersection  [ ] E7 group anagrams
BITS         [ ] F1 power of 2  [ ] F2 count bits  [ ] F3 bits 0..n  [ ] F4 reverse bits  [ ] F5 power of 4
MATH         [ ] G1 reverse int  [ ] G2 palindrome num  [ ] G3 digit sum  [ ] G4 gcd/lcm  [ ] G5 prime/sieve  [ ] G6 fact/fib  [ ] G7 fizzbuzz  [ ] G8 armstrong  [ ] G9 dec/bin  [ ] G10 trailing zeros
STRINGS      [ ] H1 reverse words  [ ] H2 LCP  [ ] H3 compression  [ ] H4 vowels  [ ] H5 remove dup  [ ] H6 rotation  [ ] H7 case  [ ] H8 strStr  [ ] H9 count-and-say
STACK        [ ] I1 valid parens  [ ] I2 min stack  [ ] I3 next greater  [ ] I4 baseball  [ ] I5 remove adj dup
PATTERNS     [ ] J1 pyramid  [ ] J2 pascal  [ ] J3 floyd  [ ] J4 diamond  [ ] J5 spiral  [ ] J6 rotate matrix  [ ] J7 tables
```

---

## 14. Answer Key — approach per problem

*(Approaches only — not full code — so you still solve them yourself. Peek only after attempting.)*

- **A1 add/carry:** right-to-left, `s = da+db+carry`, digit `s%10`, `carry = s/10`; the final leftover carry is returned/prepended.
- **A2 plus one:** go right-to-left adding 1; a digit <9 → increment and return; all 9s → set all 0 and prepend 1.
- **A3 add binary / A4 linked list:** identical carry loop, base 2 for binary; for lists, build nodes with a trailing carry node.
- **A5 multiply strings:** `product[i+j+1] += (num1[i])*(num2[j])`, carry up; skip leading zero.
- **B1/B2:** XOR everything (B2 also XOR `0..n`); the survivor is the answer. **B3:** XOR all → pick a differing bit → partition → XOR each half. **B4:** `a^=b;b^=a;a^=b`.
- **C3 Kadane:** `cur=max(x,cur+x); best=max(best,cur)`. **C4 stock:** track `minSoFar`, `best=max(best, price-minSoFar)`. **C5 majority:** Boyer–Moore vote. **C6:** track max & min (swap on negative).
- **D:** two pointers inward (D1/D2/D4/D5/D7) or slow/fast write pointer (D3/D6).
- **E:** build a frequency map / set, answer in a second pass. **E7:** sorted-string key → bucket.
- **F1:** `n>0 && (n&(n-1))==0`. **F2:** `while(n){n&=n-1;c++;}`. **F3:** `dp[i]=dp[i>>1]+(i&1)`.
- **G1 reverse int:** pop `x%10`, push `r=r*10+d`, **guard against 32-bit overflow → return 0**. **G4:** Euclid. **G5:** trial division to √n / sieve. **G10:** count 5-factors `n/5+n/25+…`.
- **H1:** split/trim/reverse/join. **H2:** vertical scan char-by-char. **H3:** run-length, only compress if shorter. **H6:** `(s1+s1).contains(s2)` with equal length. **H8:** sliding window compare.
- **I1:** stack of expected closers. **I2:** parallel min-stack. **I3:** monotonic stack. **I5:** push; pop when equal to top.
- **J2:** Pascal via sum-of-two-above. **J5 spiral / J6 rotate:** maintain four boundaries / transpose-then-reverse-rows.

---

## 15. Patterns Cheat Sheet

```
CARRY     add right→left: digit = sum%base, carry = sum/base; return the last carry
XOR       x^x=0 → XOR all to cancel pairs → odd-one-out (single/missing/swap)
ONE-PASS  keep running best/min/sum: Kadane, stock(min-so-far), majority(vote), 2nd-largest
2-POINTER ends-inward (reverse/palindrome/merge/two-sum-II) OR slow-write (move-zeros/dedupe)
FREQ MAP  count with HashMap or int[26]/int[256]; answer on a 2nd pass (anagram/first-unique/two-sum)
BITS      n&(n-1) clears lowest set bit; n&1 tests last bit; power-of-2 = n&(n-1)==0
MATH      Euclid gcd; sieve/√n prime; reverse-int WATCH 32-bit overflow; trailing-zeros count 5s
STACK     push to track matching/most-recent: valid-parens, next-greater, min-stack
```

**Universal edge-case checklist (say these every time):** empty input · single element · negatives · zero · duplicates · overflow (use `long`, guard 32-bit) · nulls · already-sorted / all-same · max/min bounds.

> **One line:** *General-round questions are a filter for speed and clean edge-case handling — group them by the eight mini-patterns (carry, XOR, one-pass, two-pointer, frequency map, bits, math, stack), solve each family together, and for every problem restate it, call out the edge cases, code cleanly, test on a tiny example, and state the complexity.*
