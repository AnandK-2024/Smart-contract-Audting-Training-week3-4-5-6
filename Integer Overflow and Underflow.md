# Integer Overflow and Underflow

An overflow/underflow happens when an arithmetic operation reaches the maximum or minimum size of a type. For instance if a number is stored in the `uint8` type, it means that the number is stored in a `8 bits unsigned` number ranging from `0 to 2^8-1`.

### Overflow
Overflow indicates that we have done a calculation that resulted in a number larger than the largest number we can represent.

Let’s look at an example involving unsigned integers.

Lets assume we have an integer stored in `1 byte`. The greatest number we can store in `one byte is 255`, so let’s take that. This is `11111111`. Now, suppose we add 2 to it to get `00000010`. The result is 257, which is 1`00000001`. The result has `9 bits`, whereas the integers we are working with consist of only 8. So the final answer is `00000001` in 8 bits.

### Underfow



