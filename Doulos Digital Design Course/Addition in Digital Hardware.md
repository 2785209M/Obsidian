![[image-13.png|405]]

This is the implementation of a full adder using logic gates. It uses two half adders, one to add a and b, the other to add the product with Cout using XOR blocks. The above shows how you can calculate the next Cout using Add blocks.

You can make different kinds of adders to improve performance:

- Carry Lookahead Adder
A carry lookahead adder performs the carry function separate to the sum function. The idea is to prevent cascading carry chains affecting system performance. They use shorter logic at the cost of being wider.

![[image-14.png|448]]
![[image-18.png|452]]

- Carry Select Adder
The carry select circuit is fast but large. It is just N ripple carry adders put together. to perform large N calculations the circuit would have to be very large. A ripple carry adder is just multiple full adders chained together.

![[image-15.png|485]]


Pipelining:
- Increases Throughput
- Increases max clock speed
- Increases  latency

Example of Latency in pipelining:
![[image-16.png|503]]


Serial Multipliers are used to multiply binary numbers. They are quite difficult to program.

It is easy to visualize the process of serial multiplication using a chimney sum.

![[image-17.png|515]]

For every cycle A shifts right by 1 and B shifts left by 1.