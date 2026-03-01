Most FPGAs now offer embedded memory blocks due to their increasing densities. These designs require very fast RAM or sometimes ROM. 

#### ROM
Read Only Memory has no write port. These are designed specifically for fast access of pre-downloaded data.

#### Single Port RAM
The simplest form of RAM. It only has one address bus which is used for both read and write operations. It may be synchronous or asynchronous but synchronous is the easy way.

#### Dual Port RAM
Dual Port RAM has separate address busses for read and write operations, which can be used simultaneously.

#### FIFO
First-In First-Out memory operates like a queue. Extra I/O ports signal when the memory is full and empty to prevent over and uner-flow. Extremely useful for applpications that require stream buffering.

#### CAMS
Contents-Addressable Memory works unusually. For write operations you supply data and an address. In read mode you only supply data and the memory will return the address where the matching data is found. Almost like you're looking up the data. Useful in network communications.

You are very likely to have to use complex functions, but at gate level it makes up more sense to use IPs. Complex functions take a very long time to make and are very complicated. It is often better to use somethinf made by someone more experiences who put in more effort, if you can afford it.

You should check whether there are pre-designed components that fit your criteria before looking into more complex options. You can chek your synthesizer, macro function libraries, and software wizards. If you can't find what you're looking for you could then look into commercial macro-functions or OP blocks, or subcontract the design to a third party.

#### Hard Macros
Contain functional information as well as bus place and route information.
#### Soft Macros
Only contain functional information
#### Parameterized Embedded Blocks
Dedicated embedded functions within FPGAs. Examples include phase-locked loops and initialization blocks.
#### Libraries
Contain implementations of complex functions.

Using Macro functions can increase productivity and performance, but at the same time, you are using somebody else's knowledge and becoming dependent on external technology.

If you are subcontracting you must follow several important guidelines.
- You must have means to asses performance
- You must specify requirements accurately
- You must own all the resources and developments
- You need quality metrics
- The subcontractor must be competent