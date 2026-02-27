![[image-10.png]]

Different conventions exist for representing numbers in binary.
Common Conventions include Two's Compliment and Gray Coding.

The first bit in two's compliment states whether the number is positive or negative.  It is called the "Sign Bit". 

![[image-11.png]]

In two's compliment positive numbers progress naturally, but negative numbers progress backwards. e.g 011 would be positive 3 but 111 would be negative 1, 110 would be negative 2, etc.

![[image-12.png]]

Gray coding works differently. There are many types of gray code but the base concept is that a change in value of 1 should correspond to a change of only one bit in a binary string at a time. 
In standard binary, adding 1 to 011 would make 100 (4). But in a Gray code adding 1 to 011 would make either 010 or 001 (both also 4) (Make sure you know what kind of gray code you are using!)  