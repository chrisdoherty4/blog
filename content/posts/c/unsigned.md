---
title: "Unsigned"
date: 2020-06-24T17:39:28-06:00

tags:
    - c
    - programming

categories:
    - Technology
    - Programming

draft: true
---

> Using an unsigned instead of an int to gain one more bit to represent positive integers is almost never a good idea

_- Bjarne Stroustrup_

## Default representation

Integer literals without any modifiers are represented as signed values in C and C++. It's natural to think in terms of signed types because we can represent negative values. Generally, everything a developer needs to do can be represented using signed integers.

Converting between signed and unsigned integers can be dangerous if done incorrectly. Even when done correctly, if the program is working with a value that can't be represented within the bounds of the target signedness the program experiences an error and that error must be dealt with.

## Unsafe conversion



## Safe(er) conversion

