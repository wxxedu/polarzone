---
title: "Lecture 03"
date: 2022-01-18T17:51:13+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms"]
---

This lecture discusses the [Big-O Notation](./lecture_03/big_o_notation) and [Binary Search](./lecture_03/binary_search).

## [Big-O Notation](./big_o_notation)

Big-O notation is used to describe the order of growth of many types of things. In [CS2040S](..), we use it to describe time complexity of algorithms. 

Note that:

- $O(f(n))$ describes the upperbound of the growth of $f$, i.e. the worst case-time complexity;

- $\Omega(f(n))$ describes the lowerbound of the growth of $f$, i.e. the best-case time complexity;

- $\Theta(f(n))$ describes is best approximation of the order of growth of $f$, i.e. the average time complexity. 

## [Binary Search](./binary_search)

Searches for an element in a sorted array in $O(log(n))$ time. 

