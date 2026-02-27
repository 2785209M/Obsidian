The AXI protocol is more sophisticated than APB which makes in better suited for high-complexity applications.

All signals in AXI are synchronous with the rising edge of the clock, apart from ARESET, which is asynchronous and, I imagine, triggered by an interrupt. However, ARESET must still be reasserted synchronously with clck.

![[image-6.png]]

In the AXI protocol multiple data lines can be active at once.

IDs link the address in one channel to the data/response of another channel.
![[image-7.png]]
![[image-8.png]]
Read and write IDs are independent. There are 2 read IDs that correspond and 3 Write IDs that correspond.

the READY signal can be asserted either before or after VALID. VALID must remain until the clock edge after READY rises. 

the READY signals are driven by the receiving component.
The VALID signals are driven by the sending component.

![[image-9.png]]

An "AXI Burst" is a set of consecutive reads or writes that form one total transaction. Each individual data transfer is called a "Beat"

The number of beats in a burst can be configured using AWLEN, ARLEN, AWSIZE, ARSIZE.