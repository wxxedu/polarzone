---
title: "CS2040S Lecture Notes - Big O Notation"
date: 2022-01-18T15:40:47+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes"]
---

We use big O notation to denote the complexities of growths of certain things. Specifically, in the context of computer algorithms, we use big O notations to measure the time complexities. Below is a function:

$$
f(x) = x^3+2x^2+3x+1
$$

suppose that we want to learn about the order of growth of the function above. We would quickly realize one thing: as $x$ gets greater and greater, the $x^3$ seemed to become the component that contributes the most to the function's growth, whereas the other components have a less and less contribution. Say, when $x=10$

$$
f(10)=1000+200+30+1=1231
$$

but when we set $x$ to $1000$,

$$
f(1000)=1,002,003,001
$$

the portion of the contribution to the growth by the $2x^2+3x+1$ is now significantly lower. If we further increase $x$, the contribution of these components can be omitted. 

## Big-O Notation

Big-O notation is defined as the follows. 

>  For functions $f$ and $g$, if there exists a value $N$ such that for any $n>N$, $f(n)<k\cdot g(n)$ holds true, then $f(n)=O(g(n))$.

This definition may be hard to grasp at first, but it essentially conveys the following things:

1. Big-O notation cares about the large values of $n$, hence, we only need to prove that there exists a value $N$ such that all the values larger than $N$ satisfies the condition;

2. **Contrary to most people's thoughts**, big-O notation cares about the **upper-bound** of the function growth. I.e., it means that the function $f$ grows at most as fast as $g$. It is **not** a close approximation of the function $f$'s growth. 

3. Because we care most about the growth, we don't need to show care about the coefficients. 

As pointed out in (2), big-O notation only cares about the upper-bound of the function growth. Similarly, we have Omega ($\Omega$) notation that denotes the lower bound of a function's growth. 

> For functions $f$ and $g$, if there exists a value $N$ such that for any $n>N$, $f(n)<k\cdot g(n)$ holds true, then $f(n)=\Omega(g(n))$.

And we also have Theta ($\Theta$) notation, which fits most people's perception of the Big-O notation, which best describes the growth of the function.

> For functions $f$ and $g$ and constants $k_{1}$ and $k_{2}$, if there exists a value $N$ such that for any $n>N$, $k_{1}\cdot g(n)<f(n)<k_{2}\cdot g(n)$ holds true, then $f(n)=\Theta(g(n))$.

## Growth Rates

Listed below are a set of growth rates, which are increasing from top to bottom. 

| Function       | Name            |
| -------------- | --------------- |
| $5$            | Constant        |
| $\log\log (n)$ | Double Log      |
| $\log (n)$     | Logarithmic     |
| $\log^{2}(n)$  | Polylogarithmic |
| $n$            | Linear          |
| $n\log(n)$     | Log-linear      |
| $n^{3}$        | Polynomial      |
| $n^{3}\log(n)$ |                 |
| $n^{4}$        | Polynomial      |
| $2^{n}$        | Exponential     |
| $2^{2n}$       |                 |
| $n!$           | Factorial       |