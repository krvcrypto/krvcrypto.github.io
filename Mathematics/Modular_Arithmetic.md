---
layout: default
mathjax: true
---

# Modular Arithmetic and Bitcoin

Modular arithmetic composes a fundamantal core of the Bitcoin system. Modular arithmetic is a system of arithmetic that limits the possible values to a certain list of numbers. We are all familar with a few such systems.  

For example, our clocks use 12 or 24 numbers. For the 12-hour clock, when the current time is 12, the next hour will be 1. We also have the days of the week. If you imagine Monday-Sunday being numbered 1-7, when we are on Sunday (7th day), the next day will be the Monday (1st day again).

Bitcoin uses such a number system.  However, instead of 12, 24, or 7, the following unfathomably large number is used:
`115792089237316195423570985008687907853269984665640564039457584007908834671663` 

(This number is 78 digits long!)

## Addition and Multiplication

Imagine it is 3 o’clock and you want to know what time it will be 20 hours from now. You can simply count 20 hours forward and you will eventually arrive at 11 o’clock. Likewise, if it is Wednesday (day 3) and you want know what day it will be in 10 days, you can count the days to arrive in the following week on Saturday (day 6). 

What we are talking about is addition (and multiplication since since multiplication is just a certain number of additions).  If we want to know what time it will be many many hours in the future, we can always count our way there.  However, using addition and multiplication like in normal arithmetic then applying modular arithmetic can provide us with shortcut tools to determine the result.

For example, image we are at 5 pm and we want to know what time it will be if we move 5 hours ahead 7 times.    If we weren't confined to 12 numbers, we'd do the following equation:
> `5 + 5*7 = 5 + 35 = 40`

However, how do you "map" it back to the 12 hour clock?  Well, if you think of what would happen if you were counting your way there, you could ask how many times do we go a complete circle around (i.e. 12 hours)?  To get that you calculate the following and drop the decimals:
> `40 // 12 = 3`   ( '//' means divide and drop decimals in some computer code)

What is left over from that division is 4, which turns out to be the resulting time because it will be those 3 full trips around the clock plus that much past the original time.  That is precisely what modular arithmetic is.  It can be defined as the remainder when dividing by a number, called the *modulus*, which is 12 in this case.   The % operator is typically used to indicate this:

> `40 % 12 = 4`

Thus, we have our answer.  If we start at 5pm and move 5 hours ahead 7 times we arrive at 4 pm.

Likewise in the underlying mathematics of Bitcoin, if an operation goes beyond that very large number, it "maps" back within the disired list of numbers using this same modular arithmetic.

## Specialness of the Number Used in Bitcoin

There is a unique property of the number used in Bitcoin.  To illustrate, imagine you repeatedly move in 2 hour increments over the clock starting at 12, say. You will get the following times: 

> 12:00, 2:00, 4:00, 6:00, 8:00, 10:00, 12:00, 2:00, ...  

Notice that it will infinitely repeat those times and the other times will never be seen. This is because 12 is divisible by 2. It eventually hits 12 again before the others. 

However, if you do the same with days of the week starting on Monday, you get the following days:  

> Monday, Wednesday, Friday, Sunday, Tuesday, Thursday, Saturday, Monday, ...  

Notice that you hit every day once before arriving at the first day again.   

This is because the number of days of the week is a prime number.  That is, the only number that divides evenly into 7 is 1.   Thus, no matter what number of days you skip, that number will not go evenly into 7 and, thus, you will never arrive at the 7th day until you do precisely 7 skips.  

That enormously large number used in Bitcoin was selected as a very very large prime number.  When a number is picked for operations in Bitcoin, it will never be restricted to certain values like in the clock example and it will be sure to be able to hit every number at least once.  Moreover, a prime number is required for some of the underlying mathematics operations in Bitcoin.

## Comparing Values
For any modulus number you pick, there will be a set of numbers that have the same remainders.  To illustrate, let's examine the values using modulus of 13.  Each of the numbers of a particular row in the following table have the same remainder when divided by 13:

| 0 | 13 | 26 | 39 | 52 | 65 | 78 | 91 | 104 | 117 |
| 1 | 14 | 27 | 40 | 53 | 66 | 79 | 92 | 105 | 118 |
| 2 | 15 | 28 | 41 | 54 | 67 | 80 | 93 | 106 | 119 |
| 3 | 16 | 29 | 42 | 55 | 68 | 81 | 94 | 107 | 120 |
| 4 | 17 | 30 | 43 | 56 | 69 | 82 | 95 | 108 | 121 |
| 5 | 18 | 31 | 44 | 57 | 70 | 83 | 96 | 109 | 122 |
| 6 | 19 | 32 | 45 | 58 | 71 | 84 | 97 | 110 | 123 |
| 7 | 20 | 33 | 46 | 59 | 72 | 85 | 98 | 111 | 124 |
| 8 | 21 | 34 | 47 | 60 | 73 | 86 | 99 | 112 | 125 |
| 9 | 22 | 35 | 48 | 61 | 74 | 87 | 100 | 113 | 126 |
| 10 | 23 | 36 | 49 | 62 | 75 | 88 | 101 | 114 | 127 |
| 11 | 24 | 37 | 50 | 63 | 76 | 89 | 102 | 115 | 128 |
| 12 | 25 | 38 | 51 | 64 | 77 | 90 | 103 | 116 | 129 |

The first row is simply the first 10 numbers that have remainder 0 when divided by 13, (i.e. all numbers that divide evenly into 13.
The second row are the first 10 numbers that have remainder 1 when divided by 13.
Similarly, the rest of the rows are the first 10 numbers that have the same remainders of 2, 3, 4, 5, all the way to 12.

The fancy mathematics way to say this is that the numbers of a particular row (that have the same remainder) are *congruent modulus* with 13.  Thus, every number in row 1 are congruent modulus 13, with a remainder of 1 when divided by 13. 

## Equations

Some amazing properties exist with various operations like addition and multiplication on congruent modulus numbers.   Particularly, in our table, if you take any number of one row and add it to any number of another row and you always take from the same 2 rows, no matter what numbers you choose the resulting number will always be also of the same row.

For example, lets pick a bunch of values from row 2 and row 5:
> 1) `2+5 = 7`

> 2) `28 + 44 = 72`

> 3) `15 + 18 = 33`

> 4) `80 + 31 = 111`

Another, from rows 1, 2, and 4:

> 5) `1 + 2 + 4 = 7`

> 6) `40 + 15 + 43 = 98`

Note that 7, 72, 33, 111, and 98 are all in the 7th row (i.e.  congruent modulus of 13, with remainder 7 when divided by 13)

The modular arithmetic applies to any number of additions, and multiplication since multiplication is simply successive additions:

> `5*1 + 2*3 = 5 + 6 = 11 `

> `18*27 + 15*29 = 486 + 435 = 921`  Note: `921 / 13 = 70` with remainder 11, thus it is would be in row 11 if there were many more numbers)

In any equation of using modular arithmetic on a particular modulus *n*, so long as each side of the equation results in a number of the same remainder (*congruent modulus n*), the equation is valid.

Observe that applying the modulus operation to any of the numbers in the equation will result in a number that is in the same row, i.e. that is *congruent modulo* *13*. So, for example, if we take equation 2 and apply the modulus operation to the second number in,  we get:
> `28 + 5 = 33`

Taking equation 4 and applying modulus operation to only the first number:
> `2 + 31 = 33`

Note, that 33 is also in row 7 and thus these equations still hold.  In fact, the order the modulus operation is applied to each number does not effect the validity of the equation.

Building upon these facts in next article, [The Bitcoin Equation](Bitcoin_Equation.html), we will see how this is applied in Bitcoin.
