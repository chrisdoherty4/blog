---
title: "Unsigned Integers"
date: 2020-06-24T17:39:28-06:00

tags:
    - c
    - c++
    - programming

categories:
    - Programming
---

> Using an unsigned instead of an int to gain one more bit to represent positive integers is almost never a good idea

_- Bjarne Stroustrup_

## Default representation

It's natural to think of integers in terms of signed types because they can represent negative values. In C/C++, integer literals, without any modifiers, are signed types.

If done incorrectly, converting between signed and unsigned integers can be dangerous. Even when done correctly, if the program is working with a value that can't be represented within the bounds of the target signedness the program experiences an error and that error must be dealt with.

It's tempting, particularly if you know you aren't going to need to represent values < 0, to use unsigned. Doing so can have dire consequences.

## Rule of thumb

> If all possible numbers can be represented in the signed type, use signed

Consider unsigned for algorithms that perform bit manipulation or with embedded systems.

