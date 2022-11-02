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

### Higher precision: IEEE 754

When modern computers store decimal numbers, they use methods to encode with precision that go beyond the integers we have seen in the previous section. A common standard is IEEE 754, which allows the encoding of positive and negative numbers that go from very large to very small. The standard has two levels of precision, *single precision* with 32 bits, also called "float", and *double precision* with 64 bits, also called "double". Each of these binary representations have three components encoded in a fixed set of binary digits:

![](https://media.geeksforgeeks.org/wp-content/uploads/Single-Precision-IEEE-754-Floating-Point-Standard.jpg)

![](https://media.geeksforgeeks.org/wp-content/uploads/Double-Precision-IEEE-754-Floating-Point-Standard-1024x266.jpg)
https://www.geeksforgeeks.org/ieee-standard-754-floating-point-numbers/

- The first bit is the **sign** $s$ of the number, with $0$ for positive and $1$ for negative
- This is followed by 8 bits (11 in double) to represent a **biased exponent** $e = E - B$ where $E$ is the binary sequence stored and $B$ is a fixed bias of value $127$ for single precision and $1023$ for double precision. This bias allows the coding to have a wider range of negative exponents so that it has high precision at the expense of not being able to represent extremely large numbers.

- The **mantissa** is the number that is exponentiated to the biased exponent. It is stored in the last 23 bits in single precision and in the last 52 bits in double precision. The mantissa is a decimal number with an implicit 1 before the dot, such that if $M$ is the binary sequence in the mantissa, the value used for the calculation is $1.M$.

This way, the number is calculated as $s * 1.M ^ {(E-B)}$

For example, the number 1|10010011|10100000000000000000000 in single precision has a value that can be calculated as:

- sign $s = 1$, i.e., a negative number
- exponent $E = 10010011_2 = 147_{10} = 127 + 20$, i.e. the unbiased exponent is 20
- mantissa $M = 1.10100000000000000000000_2 = 1 + 1 ∙ 2^{-1} + 1 ∙ 2^{-3} = 1.625_{10}$

The result is $- 1.625 ∙ 2^{20} = - 1703946.0$

### Problems with number coding

All coding systems for numbers can only encode a finite set of numbers and thus have a limited range. Computation can get out of that range and cause problems, but in practice double precision covers the vast majority of cases.

Another problem arises from limited precision. If a calculation requires precision beyond what the coding system allows, it can cause rounding errors. This is due to the limited room for decimal places. A famous example is $0.1_{10}$, which cannot be represented by a finite sequence of binary digits. Thus the representation of this numbers has small rounding errors that can become large if they accummulate over computations. Check the following code in Python:

> f = 2  
> for i in range(1, 100):  
> $\quad$ f += 10 ** (-1)  
> $\quad$   print(f)  

Produces an output with these first four lines:
>> 2.1  
>> 2.2  
>> 2.3000000000000003  
>> 2.4000000000000004

If you run its 100 iterations, you will see how these small rounding errors accumulate.

These errors can have drastic consequences. For example, during the Gulf War, a rounding error in the calculations of missile defenses of the US army led to a defense missile not being shot and an attacking Iraqi missile hit its target killing 28 people and wounding 260. This was because of a conversion of the representation of 0.1 between 24 and 48-bit representations that lead to a rounding error that, when accumulated, led to a large error in the estimation of the distance where the attacking missile would hit. You can learn more about that case in [this link](https://www-users.cse.umn.edu/~arnold/disasters/Patriot-dharan-skeel-siam.pdf). A similar error caused the explosion of the [Ariane 5 rocket in its first flight in 1996](https://www.esa.int/Newsroom/Press_Releases/Ariane_501_-_Presentation_of_Inquiry_Board_report).


This kind of errors are also important in economics and finance. In 1983, the Vancouver Stock Exchange launched an index to aggregate the value of it stocks. This index puzzled traders because it slowly lost value for no apparent economic reason, to the point that it lost 50% of its value in less than two years. This happened because trades took into account decimal digits but truncated them only for the last three decimal digits, a rounding error that sometimes produced very small loses in value but no gains. You can learn more about these kind of issues in economics in  [“The Numerical Reliability of Econometric Software”. McCullough & Vinod, Journal of Economic Literature (1999)](https://www.aeaweb.org/articles?id=10.1257/jel.37.2.633). Nowadays, computer scientists are a bit puzzled because banks use a very old programming language called COBOL (invented in 1960) for their software. The reason to use COBOL in the first place is because it does no rounding by default when doing divisions, it keeps in memory the numerator and the denominator and uses them with fractional arithmetic in the computation. This is a strong guarantee against rounding errors that has justified the dominance of COBOL for many years. Now other languages can do the same, but the systems running COBOL can be of critical necessity and a transition might be too costly.

Number coding representation issues can also appear in politics. An example is what happened when calculating the results of the April 1992 elections in the German state of Schleswig-Holstein. The Green Party had a result very close to the 5% threshold to make it into the parliament. The first results reported exactly 5.0%, but a later more careful calculation showed that the real result was 4.97%, thus leaving the Green party out of the parliament and giving the majority to the SPD. You can read a first-hand report of this case in [this link](http://catless.ncl.ac.uk/Risks/13/37#subj4).

