A bus is a set of signals joining two or more components, allowing signals to travel between them.

Bus protocols are ways for one components to request a specific piece of information from another.

A bus **transaction** is a single meaningful operation where one component requests information from another. Transactions can be composed of many signals, and communication can come from both directions.

Typically, a bus transaction with either read or write to/from an address. An address describes the location in memory where a specific piece of data 'lives'. Some protocols use different terminology such as *get* and *put*.

**Peripherals** are components that process data in unique ways, typically to communicate with the outside world. In these components each memory address serves a set purpose, and the data within those addresses have their own unique meanings in that context.

Bus architectures typically have 2 main types of component. Manager and subordinate. In most bus architectures only manager components can initiate transactions, and subordinates can only respond to them. Bus architectures usually don't allow one component to be both manager and subordinate.

