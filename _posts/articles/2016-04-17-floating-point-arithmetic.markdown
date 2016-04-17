---
layout: post
title: "Floating Point Arithmetic"
modified:
categories: articles
excerpt:
tags: []
image:
  feature:
date: 2016-04-17T19:17:56+02:00
---

My first time digging around floating piont arithmetic invariably led me to the paper [_What Every Computer Scientist Should Know About Floating-Point Arithmetic_, by David Goldberg][1]. I found the paper a great read, but I'll admit I had to go through several re-reads and references from other sources to finally grasp its contents. In this post, I give a much more gentle treatment of floating point arithmetic, inspired by one of the best books I have had the [pleasure of reading].

A natural place to start off is to look at the subset of numbers called whole numbers (referred to as integers by computer programmers) and rational numbers. Numbers such as 20, 3442 and 9992400 fall into the former category whereas 3/4, 1/3 and 7/30 are in the latter one. Rational numbers are expressed as a ratio of two whole numbers and can be equivalently expressed in fractional form - 3/4 is 0.75 for instance. The representation is depicted below:

0.75 = 1x10<sup>0</sup> + 7x10<sup>-1</sup> + 5x10<sup>-2</sup>

Digits to the left of the decimal point are multiplied by positive powers of 10 while those to the right are multiplied by negative powers of 10.
Even so, some rational numbers are not easy to represent, such as 1/3, whose fractional form is 0.3333333333..., a never-ending stream of 3's. On the other hand, irrational numbers (such as √2) do not a repeating fractional sequence.
The scientific notation is very useful for representing very large and small numbers. Expressed in this notation, the number 490,000,000 = 4.9 x 10<sup>11</sup> and 0.0000000026 = 2.6 x 10<sup>-10</sup>. The notation provides a standardized format for numbers - in these examples, 4.9 and 2.6 are the fractional part or significand, the exponent is the power to which 10 is raised.

Computers represent numbers in binary form, sequences of 1's and 0's. Floating point notation is based on the scientific notation, which we have examined for decimal numbers. So how do we represent fractional numbers in binary? Just like in the decimal number system, a binary point is used, with significand bits to the left of the binary point multiplied by negative powers of 2 and those to the right positive powers of 2. Concretely, 101.1101 is expressed as

1x2<sup>2</sup> +
0x2<sup>1</sup> +
1x2<sup>0</sup> +
1x2<sup>-1</sup> +
1x2<sup>-2</sup> +
0x2<sup>-3</sup> +
1x2<sup>-4</sup>

which is 5.8125 in decimal. In binary scientific notation, the number 101.1101 is written as 1.011101x2<sup>2</sup>, with the normalized significand greater than or equal to 1 but less than 2. This has the implication that the left of the decimal point in a significand always has a 1 in a normalized binary floating-point number.

Most computers support _IEEE Standard for Binary Floating-Point Arithmetic_, which defines single precision (requiring 4 bytes) and double precision (requiring 8 bytes) formats. The main advantage of floating-point representation over fixed-point representation is the ability to represent numbers of vastly differing magnitudes, from the radius of an atom to the distance from the earth to the sun. The precision of floating-point representation will have a bearing on how numbers are represented and the results of ensuing arithmetic operations. Single precision floating-point numbers have a 24-bit representation. What this means is that numbers differing by less than 1 part in 2<sup>24</sup> will have the same representation. Represented as single precision floating-point numbers, 16,777,216 and 16,777,217 are identical. This may, or may not be an issue but it is something the programmer ought to be aware of. Adding a very large floating-point number to a very small one or comparing two floating-point numbers may produce surprising results too. As such, one needs to be careful when writing programs that operate on floating-point numbers. One technique for instance involves sorting the numbers to be added and performing the addition in increasing order of size to ensure that small numbers reflect in the final result. Instead of comparing two floating-point numbers for equality, one can instead determine if the difference between them is less than some threshold value and base control logic on this. For instance

```
if (float1 == float2):
    # Do something
```

becomes

```
epsilon = 1.0*10^-8
if (abs(float1 - float2) < epsilon):
    # Do something
```


Further reading

What Every Computer Scientist Should Know About Floating-Point Arithmetic <https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html>


[1]: https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html
[pleasure of reading]: http://www.amazon.com/dp/0735611319/?tag=rorg-20
