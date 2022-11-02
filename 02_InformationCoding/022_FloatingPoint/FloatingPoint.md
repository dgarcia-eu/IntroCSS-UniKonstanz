---
title: "Floating Point Encoding"
subtitle: "Introduction to Computation for the Social Sciences"
author: "David Garcia"
output: html_document
---

Floating-point encoding represents real numbers in an approximate way, since it is impossible to represent infinite real numbers with a finite-length encoding. Floating-point encoding is based on an integer with a fixed precision that is multiplied by a fixed based to an integer exponent. This way you can represent many real numbers with two integer values that can be written in the memory of a computer with a predefined number of bits.


### Floating point encoding standard: IEEE 754

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

