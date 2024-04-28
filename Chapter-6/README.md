# The Link Layer
### Cyclic Redundancy Check (CRC)
CRC is used to detect errors. 
- CRC is based on binary divisors.
- There are total m + r bits. Say the polynomial function is (x^4 + x^3 + 1).
r is also known as CRC bits.
- It's impossible to detect the errors only from the pure bits. As a result,
we also send some redundant bits.
- Now one might ask what will be the number of redundant bits. The easiest
answer is to the highest number of degrees it has.
- In this case, this number is 4.

- Say, the data is 1010101010. So we add 4 more 0 to this.
- It becomes, 1010101010`0000`. Now these last 4 bits will be replaced by the
divisor.
- One might ask now, how one can find the divisor.
- We have to find the coefficient first. In this example, the coefficients
are (1.x^4+1.x^3+0.x^2+0.x^1+1.x^0)
- So the coefficient becomes 11001. Say the divisor is given in this binary
form. So to get the redundant bits from the divisor, we add n-1 bits.
- Now let's do the calculation.

1. `10101`010100000 / 11001 = `01100`. Now we borrow 1 from the right. As
calculations have to be done from a leading 1.
2. `011000`/11001 = `00001`. Now we borrow 4. (`101010`10100000)
3. `11010`/11001 = `00011`. Now we borrow 3 more.(`1010101010`0000).
4. `11000`/11001 = `00001`(`1010101010000`0).
5. 00`0010`. Since there been less than 4-bits left, we consider the last
4-bits and replace with the redundant bits.
6. 1010101010`0010`. These whole 14 bits will be sent to the receiver.

So, when the receiver receives the message, it does the same process:
1. `10101`010100010 / 11001 = `01100`. We borrow 1-bit from the right. (`101010`10100010)
2. 11000 / 11001 = 00001. We borrow 4.
3. 11010 / 11001 = 00011. We borrow 3 from a right.
4. 11001/11001 = 00000. As the remaining bit in the right is also 0. We can
conclude all the bits being 0.
5. Hence, the data is not corrupted.
6. If any of the bits in the way, changed the result would not have been all 0.


