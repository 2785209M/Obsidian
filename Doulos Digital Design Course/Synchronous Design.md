In synchronous Design every register is connected to one clock, and respond to the same type of clock edge. Data only changes once for each active edge.

Adding logic between registers can introduce delays that create errors with the register's hold time. 
Negative slack -> design is too slow and won't work at intended clock speed. 
Positive slack -> Design will work within required time slot, and should work OK

You shouldn't introduce logic onto a clock line. The clock must be perserved and uninterrupted to maintain proper system stability.

A combinational feedback loop is a loop that only contains logic. A sequential feedback loop goes around a register which prevents a breaking loop as it has to wait for the next clock edge.

D-type flip-flop - The output of the next cycle is equal to the input of this cycle

T-type flip-flop - Toggles output when input is high
![[image-1.png]]
JK Flip-Flop -  
![[image-2.png]]

RS Flip-Flop - RS stands for set and reset. If both S and R are zero, nothing changes. If S is 0 and R is 1, the register resets to zero. If S is 1 and R is 0, the register is set to 1. If both are 1 then the universe explodes.
![[image-3.png|483x201]]

Karnaugh Map - Tool for analysing and simplifying logic design

