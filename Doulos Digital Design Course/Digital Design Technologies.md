#### Integrated Circuits
IC = Integrated Circuit

The main cost for creating custom ICs comes from setting up the factory to build the ICs in the first place, which must be done before any are made. From there it's just the cost of the individual silicon wafers, which is quite small

Integrated circuits cost more to design and are more complicated, but once designed they cost less to manufacture, use less power, are more reliable, and have a smaller form factor.

Prototyping is a very good idea.

Using pre-built FPGAs removed the necessity to perform your own testing. When you manufacture integrated circuits on semiconductor wafers there is a high likelihood that many of your chips won't work, as the smallest manufacturing error on such a tiny circuit would ruin the chip.

DFT = Design for Test
BIST = Built in self test 

![[image-5.png]]
#### FPGAs

FPGA:
![[image-4.png|513x387]]


SRAM FPGA: 

| Pros                          | Cons                                                |
| ----------------------------- | --------------------------------------------------- |
| High density                  | Requires external programming at startup, e.g flash |
| Reprogrammable                | Lower reliability in safety critical situations     |
| No Hardware Programmer needed |                                                     |

Non-volatile FPGA:

| Pros         | Cons               |
| ------------ | ------------------ |
| Reliable     | Not reprogrammable |
| Confidential | Smaller Capacity   |
