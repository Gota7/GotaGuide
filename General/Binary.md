# Home
[Back To Table Of Contents](https://gota7.github.io/GotaGuide/)

# Binary
Ah, binary. The fundamental part of a computer. 1s and 0s, that's all computers are at the end of the day, just machines that read and write a bunch of 1s and 0s. But it is how these 1s and 0s are read that is what is important. Everything on screen can be represented by numbers. Colors are numbers (RGB), music is numbers (PCM), and code is numbers (ASM). So of course, in order to understand everything, you have to understand how the computer does numbers.

## How Do You Get Infinite Numbers With 2?
Simple. We have infinite numbers, and we only have 10 digits. 0, 1, 2, 3, 4, 5, 6, 7, 8, and 9. 12312 is not a digit, it's a series of digits. Like wise, we stack digits. 0 is 0 obviously. 1 is 1 of course. but for 2, we have to go to 10. I'm going to prefix binary numbers with `0b` in front from now on, as that is how they are represented in programming usually. So this means 3 is 0b11, 4 is 0b100, 5 is 0b101, and I'm sure you get the idea. There is a patter here, but what is it?

## The Pattern
Let's take the number 1234 for example. How do we represent this in terms of each individual digit and the base. We know 1 is in the thousand's place, 2 is in the hundred's place, 3 is in the ten's place, and 4 is in the one's place. This means, we can write 1234 as `1*10^3 + 2*10^2 + 3*10^1 + 4*10^0` in our current system, where 10 is the number base of our system. Likewise, to convert 0b1011 to decimal (our number system), we do `1*2^3 + 0*2^2 + 1*2^1 + 1*2^0 = 11`. Math solves everything.

## Backwards Conversion
Ok cool, I can convert to decimal, so what? How do I get back to binary?