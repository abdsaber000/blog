---
title: "Sample math note"
date: 2026-10-26
tags: [math]
excerpt: "A short note showing KaTeX equations and a figure — the pattern for new posts."
---

This post shows how math notes look on the site. Write Markdown; put LaTeX math in `$...$` or `$$...$$`.

## Inline and display math

Euler's identity: $e^{i\pi} + 1 = 0$.

The Gaussian integral:

$$
\int_{-\infty}^{\infty} e^{-x^2}\, dx = \sqrt{\pi}.
$$

## Aligned equations

KaTeX supports common environments via `aligned`:

$$
\begin{aligned}
\nabla \cdot \mathbf{E} &= \frac{\rho}{\varepsilon_0}, \\
\nabla \cdot \mathbf{B} &= 0, \\
\nabla \times \mathbf{E} &= -\frac{\partial \mathbf{B}}{\partial t}, \\
\nabla \times \mathbf{B} &= \mu_0\mathbf{J} + \mu_0\varepsilon_0\frac{\partial \mathbf{E}}{\partial t}.
\end{aligned}
$$

## Matrices

$$
A = \begin{pmatrix}
a & b \\
c & d
\end{pmatrix},
\qquad
\det A = ad - bc.
$$

## Figures

Export figures from LaTeX (or use Pandoc `--extract-media`) into `assets/images/`, then reference them like this:

![Unit circle]({{ '/assets/images/sample-note/unit-circle.svg' | relative_url }})
<p class="figure-caption">Figure: point $(\cos\theta, \sin\theta)$ on the unit circle.</p>

## Code (programming posts)

Use fenced code blocks for programming notes:

```python
def factorial(n: int) -> int:
    return 1 if n <= 1 else n * factorial(n - 1)
```

## Tip

For TikZ diagrams, custom theorem packages, or anything that must match a printed PDF exactly, put the compiled PDF on the [Handouts]({{ '/handouts/' | relative_url }}) page instead.
