<!-- ---
title: "Vieta's formulas, with AIME examples"
date: 2026-10-26
tags: [math]
excerpt: "How the coefficients of a polynomial encode symmetric sums of its roots — and three AIME problems that use this."
--- -->

Vieta's formulas turn a polynomial's coefficients into statements about its roots. On contests like the AIME, they often replace messy root-finding with a few symmetric-sum identities.

## The idea

Write a degree-$n$ polynomial in factored form and expand:

$$
a_n x^n + a_{n-1} x^{n-1} + \cdots + a_1 x + a_0
= a_n (x - r_1)(x - r_2)\cdots(x - r_n).
$$

Matching coefficients gives Vieta: the elementary symmetric sums of the roots $r_1,\ldots,r_n$ are (up to sign) ratios of coefficients.

For a **monic** polynomial $x^n + c_{n-1}x^{n-1} + \cdots + c_1 x + c_0$,

$$
\begin{aligned}
\sum_i r_i &= -c_{n-1}, \\
\sum_{i < j} r_i r_j &= c_{n-2}, \\
\sum_{i < j < k} r_i r_j r_k &= -c_{n-3}, \\
&\vdots \\
r_1 r_2 \cdots r_n &= (-1)^n c_0.
\end{aligned}
$$

(The sign in front of each symmetric sum alternates: $-$, $+$, $-$, $\ldots$.)

### Quadratic and cubic (the ones you use constantly)

For $x^2 + px + q = 0$ with roots $r,s$:

$$
r + s = -p, \qquad rs = q.
$$

For $x^3 + ax^2 + bx + c = 0$ with roots $r,s,t$:

$$
\begin{aligned}
r + s + t &= -a, \\
rs + rt + st &= b, \\
rst &= -c.
\end{aligned}
$$

If the leading coefficient is not $1$, divide through by it first (or replace each coefficient $c_k$ by $c_k / a_n$).

### Power sums from Vieta

Often you need $\sum r_i^2$ or $\sum r_i^3$. Convert using

$$
\sum r_i^2 = \Bigl(\sum r_i\Bigr)^2 - 2\sum_{i<j} r_i r_j,
$$

and for cubes,

$$
\sum r_i^3 = \Bigl(\sum r_i\Bigr)^3 - 3\Bigl(\sum r_i\Bigr)\Bigl(\sum_{i<j} r_i r_j\Bigr) + 3\sum_{i<j<k} r_i r_j r_k.
$$

(These are Newton–Girard identities in disguise.)

---

## Example 1 — 1996 AIME, Problem 5

> Suppose that the roots of $x^3 + 3x^2 + 4x - 11 = 0$ are $a$, $b$, and $c$, and that the roots of $x^3 + rx^2 + sx + t = 0$ are $a+b$, $b+c$, and $c+a$. Find $t$.

**Vieta on the first cubic:**

$$
a + b + c = -3, \qquad ab + bc + ca = 4, \qquad abc = 11.
$$

The constant term of the second monic cubic is

$$
t = -(a+b)(b+c)(c+a).
$$

Expand the product via the identity

$$
(a+b)(b+c)(c+a) = (a+b+c)(ab+bc+ca) - abc.
$$

Substitute:

$$
(a+b)(b+c)(c+a) = (-3)(4) - 11 = -23,
$$

so $t = -(-23) = 23$.

**Answer:** $023$.

The same identity is worth memorizing: whenever new “roots” are pairwise sums of old roots, the product collapses to $(\sum r)(\sum rs) - \prod r$.

---

## Example 2 — 2003 AIME II, Problem 9

> Let
> $$
> P(x) = x^6 - x^5 - x^3 - x^2 - x, \qquad
> Q(x) = x^4 - x^3 - x^2 - 1.
> $$
> If $z_1,z_2,z_3,z_4$ are the roots of $Q(x)=0$, find $P(z_1)+P(z_2)+P(z_3)+P(z_4)$.

You do **not** find the $z_i$. For each root of $Q$,

$$
z^4 = z^3 + z^2 + 1.
$$

Reduce higher powers of $z$ with this relation:

$$
\begin{aligned}
z^5 &= z\cdot z^4 = z^4 + z^3 + z = 2z^3 + z^2 + z + 1, \\
z^6 &= z\cdot z^5 = 2z^4 + z^3 + z^2 + z = 3z^3 + 3z^2 + z + 2.
\end{aligned}
$$

Then

$$
\begin{aligned}
P(z)
&= z^6 - z^5 - z^3 - z^2 - z \\
&= (3z^3 + 3z^2 + z + 2) - (2z^3 + z^2 + z + 1) - z^3 - z^2 - z \\
&= z^2 - z + 1.
\end{aligned}
$$

So

$$
\sum_i P(z_i) = \sum_i z_i^2 - \sum_i z_i + 4.
$$

From $Q(x) = x^4 - x^3 - x^2 - 1$ and Vieta,

$$
\sum z_i = 1, \qquad \sum_{i<j} z_i z_j = -1,
$$

hence

$$
\sum z_i^2 = 1^2 - 2(-1) = 3.
$$

Therefore

$$
\sum P(z_i) = 3 - 1 + 4 = 6.
$$

**Answer:** $006$.

Pattern: *evaluate a polynomial at the roots of another* $\Rightarrow$ reduce with the second polynomial, then apply Vieta to what remains.

---

## Example 3 — 2005 AIME I, Problem 8

> The equation
> $$
> 2^{333x-2} + 2^{111x+2} = 2^{222x+1} + 1
> $$
> has three real roots. Their sum is $m/n$ in lowest terms; find $m+n$.

Substitute $y = 2^{111x}$. Then

$$
2^{333x-2} = \tfrac14 y^3, \qquad
2^{111x+2} = 4y, \qquad
2^{222x+1} = 2y^2,
$$

and the equation becomes

$$
\frac14 y^3 + 4y = 2y^2 + 1
\quad\Longleftrightarrow\quad
y^3 - 8y^2 + 16y - 4 = 0.
$$

Let the positive roots be $y_1,y_2,y_3$ (each $y = 2^{111x} > 0$). Vieta gives

$$
y_1 y_2 y_3 = 4.
$$

The corresponding $x$-roots satisfy $y_i = 2^{111 x_i}$, so

$$
\sum_i x_i
= \frac{1}{111}\sum_i \log_2 y_i
= \frac{1}{111}\log_2(y_1 y_2 y_3)
= \frac{1}{111}\log_2 4
= \frac{2}{111}.
$$

Thus $m+n = 2 + 111 = 113$.

**Answer:** $113$.

Pattern: exponential equations with commensurate exponents often become polynomials after a substitution $y = a^{kx}$; Vieta then reads off products (hence sums of logs) without solving for each root.

---

## Quick checklist for AIME

1. **Make the polynomial monic** (or track the leading coefficient carefully).
2. **Match the sign pattern**: sum of roots is *minus* the $x^{n-1}$ coefficient for a monic polynomial.
3. Prefer **symmetric expressions** ($\sum r$, $\sum rs$, $\prod r$, $\sum r^2$, …) over individual roots.
4. If new quantities are built from old roots (sums, products, $r+\tfrac1r$, …), rewrite them in elementary symmetric sums, then apply Vieta.
5. Complex roots count — Vieta does not care whether roots are real.

---

## Practice

- **1995 AIME Problem 5.** A quartic with four non-real roots; the product of two is $13+i$ and the sum of the other two is $3+4i$. Find the coefficient of $x^2$.
- **2014 AIME I Problem 6.** A cubic with integer coefficients and a given root; use Vieta (and rationality) to constrain the others.
- Revisit any AIME cubic/quartic whose question asks for a *symmetric* function of the roots — that is almost always Vieta.

The three worked problems above already cover the main contest moves: transforming roots, reducing one polynomial modulo another, and reading a sum of logs from a product of substituted roots.
