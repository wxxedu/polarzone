---
title: "Data Representation"
date: 2022-01-19T16:00:23+08:00
draft: false
series: ["CS2100"]
tags: ["CS2100", "Computer Organization", "Binary", "Data Representation", "ASCII"]
---

Module: [CS2100](../..)

Lecture: [Lecture 03](..)

There are four basic data types in C. Namely, they are:

- `int` - 4 bytes long, which is equivalent to 32 bits. 
- `float` - 4 bytes long, which is equivalent to 32 bits.
- `double` - 8 bytes long, which is equivalent to 64 bits.
- `char` - 1 byte long, which is equivalent to 8 bits.

In this article, we are going to explore the underlying data structure that represet the 4 data types in C. What represents `int`, what represents `char`, and what represents `float` and `double`.

## Different Base Systems

### Decimal System

As we all know, we live in a world which uses a decimal system. You have symbols 0 to 9, and if you happen to add 1 to 9, you get 10, which clears out the current digit and adds 1 to the next digit. This is not as trivial as it seems. When we say the number 123.45, what exactly do we mean? It turns out that we mean $$1\times 10^{2}+2\times 10^{1}+3\times 10^{0}+4\times 10^{-1}+5\times 10^{-2}$$
this expression, of course, gives us 123.45. This is apparent because we are using a decimal system. 

### Other Systems

However, there are more systems than the decimal system. As you probably know, there is the binary system that modern computers operate on. There's also the hexadecimal system which you've probably encountered if you happen to enter the hex values for a color in a design software. And of course, you can also use systems with any base.

### Conversions to Decimal

Just like the expression above, we can convert other systems to the decimal system using the formula:

$$
\text{Number}\_{10}=\sum\text{Number}\_{R}[i]\times R^{i}
$$

Note that $i$ starts from the decimal point to the left. 

For example, if we have number 10101101, we can convert it to the decimal system like this:

$$
\begin{align*}
&10101101_{2}\\\\ 
=&1\times 2^{7}+0\times 2^{6}+1\times 2^{5}+0\times 2^{4}\\\\ &+1\times 2^{3}+1\times 2^{2}+0\times 2^{1}+1\times 2^{0}\\\\
=& 128 + 32  + 8 + 4 + 1\\\\
=&173_{10}
\end{align*}
$$

This formula works for other systems as well. Say, if you have 74 in an oxadecimal system, and you want to convert it to the decimal system, you simply do this:

$$
74_{8}=7\times 8^{1}+4\times 8^{0}=60_{10}
$$

### Conversions From Decimal

Now, what if you want to convert between non-decimal systems? Well, one way of doing so is to convert to a decimal system and then convert back to the other system. To know how to convert a number from a decimal system to a non-decimal system. Say, we have a number 123.45, what is the binary value of this system?

Well, as you can probably observe, the number consists of two parts, the integer before the decimal point, and the decimal after the decimal point. We need to separate these and take a look at them independently. 

#### Integer Portion

For number 123, we need to divide it by 2 consecutively, and take note of the remainder. 

| Expression  | Result | Remainder |
| ----------- | ------ | --------- |
| $123\div 2$ | $61$   | $1$       |
| $61\div 2$  | $30$   | $1$       |
| $30\div 2$  | $15$   | $0$       |
| $15\div 2$  | $7$    | $1$       |
| $7\div 2$   | $3$    | $1$       |
| $3\div 2$   | $1$    | $1$       |
| $1\div 2$   | $0$    | $1$       |

Here, we read from the bottom up, and we have: 1111011, which, if we convert back, equates 123. 

#### Decimal Portion

What about the decimal portion then?

Well, it's also simple. We solve the decimal portion by multiplying the decimal portion by 2, find out the decimal portion, multiply the decimal portion by 2, find out the decimal portion ... so on and so forth, until we reach 1. Sometimes, it is not possible for us to reach 1, that means we cannot accurately represent the number in a base 2 system. It will be an approximation.

| Expression       | Result | Integer |
| ---------------- | ------ | ------- |
| $0.45 \times  2$ | $0.9$  | $0$     |
| $0.9\times  2$   | $1.8$  | $1$     |
| $0.8\times  2$   | $1.6$  | $1$     |
| $0.6\times  2$   | $1.2$  | $1$     |
| $0.2\times  2$   | $0.4$  | $0$     |
| $0.4\times  2$   | $0.8$  | $0$     |
| $0.8\times  2$   | $1.6$  | $1$     |

In the table above, we read from top down. Hence, the binary value for the table above would be $0111001$. 

Therefore, the number, represented in the binary form, would be 

$$1111011.0111001$$

## Char Representation

Character Representation uses the ACII table.

| LSBs\\MSBs | 000 | 001      | 010 | 011 | 100 | 101 | 110 | 111 |
| ---------- | --- | -------- | --- | --- | --- | --- | --- | --- |
| 0000       | NUL | DLE      | SP  | 0   | @   | P   | \`  | p   |
| 0001       | SOH | DC$_{1}$ | !   | 1   | A   | Q   | a   | q   |
| 0010       | STX | DC$_{2}$ | "   | 2   | B   | R   | b   | r   |
| 0011       | ETX | DC$_{3}$ | #   | 3   | C   | S   | c   | s   |
| 0100       | EOT | DC$_{4}$ | $   | 4   | D   | T   | d   | t   |
| 0101       | ENQ | NAK      | %   | 5   | E   | U   | e   | u   |
| 0110       | ACK | SYN      | &   | 6   | F   | V   | f   | v   |
| 0111       | BEL | ETB      | '   | 7   | G   | W   | g   | w   |
| 1000       | BS  | CAN      | (   | 8   | H   | X   | h   | x   |
| 1001       | HT  | EM       | )   | 9   | I   | Y   | i   | y   |
| 1010       | LF  | SUB      | *   | :   | J   | Z   | j   | z   |
| 1011       | VT  | ESC      | +   | ;   | K   | \[  | k   | \{  |
| 1100       | FF  | FS       | ,   | <   | L   | \\  | I   | \|  |
| 1101       | CR  | GS       | -   | =   | M   | \]  | m   | \}  |
| 1110       | O   | RS       | .   | >   | N   | ^   | n   | \~  |
| 1111       | SI  | US       | /   | ?   | O   | \_  | o   | DEL    |

MSB means the most significant bits, whereas LSB  represent the list significant bits.

Let's take the character for 0 for example: note that this is the character 0 but not the 

## Integer Representation

Now the problem arises: how to we actually represent an integer in the computer?

There are two types of integers: signed and unsigned. When the integer is unsigned, we could just represent it with the binary value of the value that we want to represent -> pretty easy!

However, what about when we want to represent the integers that are signed? Intuitively, this would bring us to the idea of a **sign and magnitude** representation. 

### Sign and Magnitude

Recall how we represent the decimal numbers, we put an optional `+` sign in front of the positive numbers, and a mandatory `-` sign in front of the negative numbers. Then, we use the remaining the digits to represent the numerical value, or the magnitude, of the number. 

The sign and magnitude way of representation uses a similar approach here. It uses the first bit (the most significant bit) to represent the sign. By convention, we use 1 to represent negative numbers and 0 to represent positive numbers. Hence, the number -127 could be represented by:

$$
\color{red}{1}\color{black}1111111
$$

the first 1 is marked red because it represents the negation sign, whereas the remaining 7 1s represent the number 127.

However, this way of data representation has its inherent flaw a bit problem: the addition and subtraction operations behave differently on positive and negative numbers. 

Suppose that you want to add 1 to 0, what do you do? Intuitively, you would want to write down 00000000 + 1 = 00000001. But, is that the case here? No! If you do so, you will get negative 1 as opposed to positive 1. You are actually minus 1. So you see, by adding 1 to the last bit behaves differently on positive and negative numbers if we use this intuitive representation. So, what are the ways that we could do? Here comes the 1s representation.

### 1s Component

So here comes another way of representing the integers! Suppose we have a number $x$ that could be represented in an $n$-bit integer, the negated value of $x$ could be represented by:

$$-x=2^{n}-1-x$$

Let's take $n=4$ as an example. Here, $2^{4}-1=10000_{2}-1=1111_{2}$. So, if we take $x=2$, we will have $-0010_{2}=16-1-2=13=1101_{2}$. Essentially, we are using $1111-0010$ to find out the negation of $0010$.

We call this the 1s component approach to represent the numbers. The good thing about this approach is that it still keeps the sign bit: the number will always be negative if the most significant bit is 1, and the number will always be positive if the most siginificant bit is 0. It's also really easy to calculate. Essentially, you only need to take the XOR operation for each bit of the integer and 1111. 

What's more, when the number gets bigger than the largest possible number, in this case 0111, which represents 7, it would go to 1000, which is -7. This means that both positive numbers and negative numbers are handled the same way.

The problem with this approach is that it has two 0s. Both $1111$ and $0000$ represent 0. Hence, this this causes a little bit trouble if your calculation were to traverse through 0. 

### 2s Component

To solve the limitation of the 1s component approach, we can adjust this by using the 2s approach. What this does is that it inverses the bits and add 1 to the least significant bit. When represented in the decimal system, the algorithm looks like this:

$$-x=2^{n}-x$$

Again, let's take a look at the example when when $n=4$. Hence, if $x=7$, $-x$ would be represented by $-0111_{2}=1000_{2}+1_{2}=1001_{2}$. 

The 2s component solves the problem of duplicating 0s, and now we have a great represent system for integers. 

### `int` in C

The `int` in C has 32 bits, which means that it ranges from $2^{31}-1$ to $-2^{31}$.

## Real Number Representation

In the [previous section](#Integer%20Representation), we talked about the representation of decimals using a fixed decimal point. Similarly, when we try to represent real numbers, we can used a fixed-point representation. Say we have 8 bits, then we can use the first 4 bits to represent the integer part of the number, whereas the last 4 bits could be used to represent the decimal part of the number. 

However, this approach is not good in that it is too rigid. Sometimes when we want to represent a number with large integer but few to no decimal digits; other times we want to represent a number with large number of decimal digits but has few or no integer digits. Here comes the **IEEE754 Floating-Point representation**. Instead of using a fixed point, it divides the bits into three parts. 

The Sign Bit: the most significant bit, and it is used to represent whether if the number is positive or negative. 

The Exponent: the next 8 bits after the sign bit, and it is used to represent the exponent of the 2 that the mantisa multiplies by. Note that this number is in excess notation with an excess of 127, meaning that it starts with 00000000 representing -127, and 10000000 represents 1.

The Mantisa: the fraction portion. It consists of 23 bits, and can be converted to decimal using the method described [above](#Decimal%20Portion). 

For example, if we were to represent 17.25 in the floating point number, we first need to figure out the sign bit. In this case, because 17.25 is a positive number, the sign bit is 1.

Then, we need to figure out the mantisa and the exponent. 17.25 when converted to binary is 10001.01. Here, 

$$
1001.01_{2}=1.00101_{2}\times 2^{3}
$$

Hence, the mantisa of this starts with $00101$, and should be 

$$
00101 00000 00000 00000 00
$$

Note that because the binary will always start with 1, we don't have to include that first 1, and we only need the decimal portions of the binary. 

The exponent portion should  represent $3$. Because it is in excess notation and has an excess of 127, we need to add 3 to 127 which is 130, and convert it to binary. In this case, it is $1000 0010$. Here, the entire floating point representation for this should be:

$$
01000001000101 00000 00000 00000 000
$$
