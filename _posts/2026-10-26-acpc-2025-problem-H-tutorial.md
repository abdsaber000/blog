---
title: "Count The Number of Partitions (ACPC 2025 Problem H)"
date: 2026-10-26
tags: [math, programming]
excerpt: "We discuss the solution to integer partitioning problem."
---

In this blog, we discuss the solution to ACPC 2025 problem H. This problem had one of the fewest solves in the contest (only one team got it right). At first glance it might look like a convolution problem, but it has a simple dynamic programming solution. Here we work through one of several possible approaches.

![ACPC 2025 standings]({{ '/assets/images/acpc-2025/acpc-2025-standings.png' | relative_url }})
<p class="figure-caption">Figure: ACPC 2025 standings (problem H was solved by only one team).</p>

## The problem

Given an integer $n$, count the number of sequences $a$ of length $m$ with $1 \leq m \leq n$ such that

$$
\begin{aligned}
&a_1 \leq a_2 \leq \cdots \leq a_m \\
&a_1 + a_2 + \cdots + a_m = n
\end{aligned}
$$

and every even value appears at most once in the sequence. (Odd values may repeat freely.) Output the answer modulo $10^9+7$.

For $n = 4$, the valid sequences are

$$
[1,1,1,1] \quad [1,1,2] \quad [1,3] \quad [4]
$$

The sequence $[2,2]$ is invalid: $2$ is even and appears twice.

Because the sequence is required to be non-decreasing, each valid sequence is uniquely determined by how many times we used each element. Two sequences are different if we used an element different number of times in one sequence than the other.

## Splitting into odd and even parts

The restriction applies only to even numbers. That suggests splitting any valid sequence into two pieces:

- the **odd** parts (which may repeat), and
- the **even** parts (which must all be distinct).

If the odd parts sum to $k$ and the even parts sum to $n-k$, we can count those two pieces separately and combine them. Define:

- $o_k$ = number of ways to write $k$ as a sum of odd positive integers,
- $e_k$ = number of ways to write $k$ as a sum of **distinct** even positive integers,
- $d_k$ = number of ways to write $k$ as a sum of **distinct** positive integers (odd or even).

The answer for $n$ is then the convolution

$$
\sum_{k=0}^{n} o_k \, e_{n-k}
$$

The rest of the note shows that both $o$ and $e$ reduce to the single sequence $d$.

## Even parts

Fix a sum $i$, and let $a_1, a_2, \ldots, a_m$ be distinct even positives with

$$
a_1 + a_2 + \cdots + a_m = i.
$$

Every even integer can be written as $a_j = 2b_j$ where $b_j$ is an integer (even or odd). We notice that because all $a_j$ are pairwise distinct, then all $b_j$ are, and

$$
\begin{aligned}
&2b_1 + 2b_2 + \cdots + 2b_m = i \\
&b_1 + b_2 + \cdots + b_m = \frac{i}{2}
\end{aligned}
$$

So sequences of distinct even parts summing to $i$ can be mapped to sequences of distinct (even or odd) parts summing to $i/2$.

Two immediate consequences:

1. If $i$ is odd, then $i/2$ is not an integer, so no such sequence exists and $e_i = 0$.
2. If $i$ is even, the count is exactly $d_{i/2}$.

In other words,

$$
e_i =
\begin{cases}
d_{i/2} & \text{if } i \text{ is even} \\
0 & \text{if } i \text{ is odd}
\end{cases}
$$

## Odd parts

It remains to handle $o_i$, the number of partitions of $i$ into (not necessarily distinct) odd parts. In fact, this is equivalent to the number of partitions of $i$ into distinct parts, which is $d_i$:

$$
o_i = d_i.
$$

One clean proof uses generating functions. Let $f$ be the generating function for $d$ and $g$ be the generating function for $o$. We can express the generating function as:

$$
\begin{aligned}
f(x) &= (1+x)(1+x^2)(1+x^3)\cdots \\
g(x) &= (1+x+x^2+\cdots)(1+x^3+x^6+\cdots)(1+x^5+x^{10}+\cdots)\cdots
\end{aligned}
$$

The generating function for $f$ means that for every number, we either take the number or leave it, which ensures that all numbers in the sequence are distinct. While in the generating function of $g$, we can either take the number, or one of its multiples, or leave it.

In product form, the same functions are

$$
\begin{aligned}
f(x) &= \prod_{k=1}^{\infty} (1 + x^k) \\
g(x) &= \prod_{j=0}^{\infty} \bigl(1 + x^{2j+1} + x^{2(2j+1)} + x^{3(2j+1)} + \cdots\bigr)
\end{aligned}
$$

The factor for each odd part $2j+1$ in $g$ is a geometric series: we may use that part $0,1,2,\ldots$ times. Recall the geometric series identity

$$
\frac{1}{1 - y} = 1 + y + y^2 + y^3 + \cdots,
\qquad \lvert y \rvert < 1
$$

Substituting $y = x^{2j+1}$ gives

$$
1 + x^{2j+1} + x^{2(2j+1)} + \cdots = \frac{1}{1 - x^{2j+1}}
$$

Hence

$$
g(x) = \prod_{j=0}^{\infty} \frac{1}{1 - x^{2j+1}} = \frac{1}{1-x} \cdot \frac{1}{1-x^3} \cdot \frac{1}{1-x^5} \cdots
$$


For $f$, multiply and divide each factor by a conjugate. Using $1 - x^{2k} = (1 - x^k)(1 + x^k)$, we get

$$
1 + x^k = \frac{1 - x^{2k}}{1 - x^k}
$$

and therefore

$$
\begin{aligned}
f(x)
&= \prod_{k=1}^{\infty} (1 + x^k)
= \prod_{k=1}^{\infty} \frac{1 - x^{2k}}{1 - x^k} \\
&= \frac{1-x^2}{1-x} \cdot \frac{1-x^4}{1-x^2} \cdot \frac{1-x^6}{1-x^3} \cdot \frac{1-x^8}{1-x^4} \cdots
\end{aligned}
$$

Look at the numerator and the denominator separately:

- The numerator is $\prod_{k=1}^{\infty} (1 - x^{2k})$, so the exponents that appear are exactly the even positives $2,4,6,\ldots$
- The denominator is $\prod_{k=1}^{\infty} (1 - x^k)$, so the exponents that appear are all positives $1,2,3,\ldots$

Every term in the denominator with even exponent cancels the matching term in the numerator. What remains in the denominator are the terms with odd exponent only:

$$
f(x) = \frac{1}{1-x} \cdot \frac{1}{1-x^3} \cdot \frac{1}{1-x^5} \cdots = g(x)
$$

Since the generating functions are equal, their coefficients are equal: $o_i = d_i$ for every $i$.

## Combining the results

We now have $o_k = d_k$ and $e_{n-k}$ expressed in terms of $d$ as well, so the answer depends only on the sequence $d$:

$$
\sum_{k=0}^{n} o_k \, e_{n-k}
= \sum_{k=0}^{n} d_k \, e_{n-k},
\qquad
e_j =
\begin{cases}
d_{j/2} & j \text{ even} \\
0 & j \text{ odd}
\end{cases}
$$

It remains to compute $d_0, d_1, \ldots, d_n$ (the number of partitions of each integer into distinct parts) and then evaluate the sum above.

## A bound for the sequence length

Note that a partition into distinct positive parts cannot be too long. The shortest $k$ distinct positives are $1,2,\ldots,k$, and their sum is the triangular number

$$
1 + 2 + \cdots + k = \frac{k(k+1)}{2}.
$$

Since this summation cannot exceed $n$, solving $\frac{k(k+1)}{2} \le n$ gives the quadratic

$$
\begin{aligned}
k^2 + k - 2n \le 0 
\quad \Rightarrow \quad k \le \frac{-1 + \sqrt{1 + 8n}}{2}
\end{aligned}
$$

For large $n$, the max value of $k$  $\approx$ $\sqrt{2n}$, so the maximum number of parts we ever need is $O(\sqrt{n})$.

## Defining the DP state

Let $\mathrm{dp}[k][s]$ be the number of sequences of $k$ distinct positive integers that sum to $s$. Then

$$
d_s = \sum_{k} \mathrm{dp}[k][s]
$$

where the sum runs over all possible lengths $k$ (at most $O(\sqrt{s})$, by the bound above).

To get a recurrence, take such a sequence $a_1 < a_2 < \cdots < a_k$ with

$$
s = a_1 + a_2 + \cdots + a_k
$$

Subtract $1$ from every term:

$$
s - k = (a_1 - 1) + (a_2 - 1) + \cdots + (a_k - 1)
$$

The new terms are still strictly increasing, hence still distinct. What happens next depends on whether $a_1 = 1$:

- If $a_1 \ge 2$, then every $a_i - 1$ is still a positive integer. We obtain a sequence of length $k$ summing to $s - k$, so there are $\mathrm{dp}[k][s - k]$ such sequences.
- If $a_1 = 1$, then $a_1 - 1 = 0$, which we drop. What remains is a sequence of length $k - 1$ of distinct positive integers summing to $s - k$, so there are $\mathrm{dp}[k - 1][s - k]$ such sequences.

Since there's no overlapping between the 2 cases, we can simply sum the results. Therefore

$$
\mathrm{dp}[k][s] = \mathrm{dp}[k][s - k] + \mathrm{dp}[k - 1][s - k]
$$

The base case for this recurrence is $\mathrm{dp}[0][0] = 1$ since there's only $1$ way to build an empty sequence.

One more observation: the recurrence for length $k$ only reads values from the current row $k$ and the previous row $k-1$. So we only need to store two arrays of length $n+1$, which reduces the memory from $O(n\sqrt{n})$ to $O(n)$ while keeping the same $O(n\sqrt{n})$ time.

## Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1e9 + 7;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;

    int K = (int)((-1 + sqrt(1.0 + 8.0 * n)) / 2) + 1;
    vector<int> prev(n + 1), cur(n + 1), d(n + 1);
    prev[0] = 1;
    d[0] = 1;
    for (int k = 1; k <= K; k++) {
        fill(cur.begin(), cur.end(), 0);
        for (int s = k; s <= n; s++) {
            cur[s] = (cur[s - k] + prev[s - k]) % MOD;
        }
        for (int s = 0; s <= n; s++) {
            d[s] = (d[s] + cur[s]) % MOD;
        }
        prev.swap(cur);
    }

    int ans = 0;
    for (int k = 0; k <= n; k++) {
        int j = n - k;
        if (j % 2 == 0) {
            ans = (ans + 1LL * d[k] * d[j / 2]) % MOD;
        }
    }
    cout << ans << "\n";
}
```

## Remarks

- Via generating functions, this problem is equivalent to counting partitions in which every part appears at most three times. Reaching an $O(N\sqrt{N})$ solution without that simplification is substantially harder.

- There is another approach based on Euler's pentagonal number theorem. It has the same time complexity as the solution above.