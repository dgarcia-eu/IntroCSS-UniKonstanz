---
title: "Information Coding Basics"
subtitle: "Introduction to Computation for the Social Sciences"
author: "David Garcia"
output: html_document
---


### Positional Number Systems

Positional number systems represent numbers with sequences of digits. The value represented by each digit depends of its position in the sequence for the number. The list of possible digits has to be finite and is known as alphabet. For example, in the decimal system there are ten digits (0-9) that form its alphabet.

Each positional number systems has a special value called the **base** (or radix). Each position adds a value to the number corresponding to the digit multiplied by the base to the k-th power, where k is the position indexed starting with zero. For example, a number in in base $b$ with four digits has the right-most digit value multiplied to $b^0$, the one to the left of it to $b^1$, the next to the left to $b^2$, and the right-most to $b^3$. This way, for the number $12$ in base 10, the $1$ is multiplied by $10^1$ and for the number $1200$ in base 5, the $1$ is multiplied by $5^3$.

Typical bases used in computer science are:

- **base 10:** decimal notation (digits 0,1,2,3,4,5,6,7,8,9)
- **base 2:** binary notation (digits 0,1)
- **base 8:** octal notation (digits 0,1,2,3,4,5,6,7)
- **base 16:** hexadecimal notation (0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F)

The convention to indicite the base of a number, if not clearly known, is to use a subscript with the base. For example $7$ in decimal is $7_{10}$ which has the same value as $7_8$ and the same as $110_2$. However, $9_{10}$ in octal is $11_{8}$ and $1001_2$ in binary.

### Conversion Between Number Systems

Comparing numbers between systems might not be straightforward and you could have to do some calculations. For example, which is greater, $47_8$ or $43_{10}$?

You can do the comparison by converting one number to the other base. For example, the octal number $47_8$ can be calculated in decimal as $47_8 = 7 * 8^0 + 4*8^1 = 39_{10}$, which is less than $43_{10}$. You could also do it by converting $43_{10}$ to octal notation as $43_{10}=5*8^1 + 3*8^0 = 53_8$ which is larger than $47_8$.

Some conversions are easier than others. For example, converting between binary and octal is easy because each octal digit corresponds to exactly three binary digits. This way conversion can be done just by translating digits in sequence. Using a table like this one:

octal | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
--|--|--|--|--|--|--|--|--|
binary | 000 | 001 | 010 | 011 | 100 | 101 | 110 | 111 |

You can easily translate $47_8$ as $100$ $111_2$ sine $100$ is the code for $4$ on the table and $111$ is the code for $7$. Also the conversion between hexadecimal (base 16) and binary is easy, where each group of four binary digits map to one hexadecimal digit.

Converting between any two bases is not as simple but can be done. Converting from a positive number $n$ from decimal to binary is the task solved by our first algorithm:

> *Step 1: $k$ = 0*  
> *Step 2: $x_k$ = $n$ mod 2*  
> *Step 3: $n$ = ⌊$n$/2⌋*  
> *Step 4: $k$ = $k + 1$*  
> *Step 5: if $n$>0 go to Step 2*  
> *Step 6: return $x_k$ ... $x_0$*  

The algorithm starts with the number in $n$ and it iteratively divides it by 2, taking note of the reminder (also called rest or the result of modulo 2) and the result of the division. The sequence of reminders forms the binary digit starting from the right-most binary digit. Variable $k$ keeps track of the position of the digits and when the division gives zero as a result, the binary number is ready. You can see this as it happens in this example to convert $190_{10}$ to binary:

190 : 2 = 95, rest: 0  
95 : 2 = 47, rest: 1  
47 : 2 = 23, rest: 1  
23 : 2 = 11, rest: 1  
11 : 2 = 5, rest: 1  
5 : 2 = 2, rest: 1  
2 : 2 = 1, rest: 0  
1 : 2 = 0, rest: 1  

As you see, the result of one division is the number that is divided in the next step. The rests can be read from the first to the last as the number written from the right to the left, thus $190_{10} = 10111110_{2}$.

You can generalize the algorithm to the conversion from decimal to any base $b$ by replacing the 2 in the modulo operation of Step 2 and of the division in Step 3 for the base $b$. Also decimal numbers can be converted this way, check the course slides for the algorithm and an example.
).

