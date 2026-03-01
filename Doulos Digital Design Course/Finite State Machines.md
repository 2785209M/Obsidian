A finite state machine represents a system that has a defined state at every given moment, can switch between states depending on its inputs, and produces outputs which depend on the current state (and sometimes the inputs).

Finite state machines are at the heart of an efficient system. They are faster, maintainable, efficient, and reliable. However to create properly you need a good understanding of the problems.

![[image-19.png]]

#### Moore
A Moore type state machine is a state machine in which the outputs depend directly on the current state, and only change when the current state changes.

#### Mealy
A Mealy type state machine is a machine in which the output is driven by both the current state and the type of input. E.g. the output of the system can change with a change of condition in the input, without a steady state change in the system.

It is necessary to specify a default state in the system when there is an absence of meaningful input.

#### Race Conditions
Race conditions occur when two or more conditions are asserted at the same time, and the system does not know which one has a higher priority. In some design tools priority can be set using numbers. A lower number means  a lower priority.

Finite state machines can be represented easily in HDLs using case state blocks.

![[image-20.png]]


States can be represented using binary. In order to have 3 states you would need 3 flip-flops as they can count to 3 in binary (11), to count up to 7 you would need 3 flip-flops, etc.

![[image-21.png]]

You can also use a method called one-hot encoding in which each flip flop indivudualy represents a specific state. You need more flip flops but it leads to less combinational logic.

![[image-22.png]]


#### FSM Design Rules
- Finite state machines CANNOT have asynchronous Inputs.
- You must deal with unspecified states.
- You must have a reset, default state, and startup conditions
- You must use appropriate encoding
- You must avoid Deadlock
- You must avoid Race conditions
- You must verify your design thoroughly
