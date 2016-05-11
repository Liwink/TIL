## What the problem process solves

When the system runs multiple programs at once, the programs should share the CPU. 

How to make the system easy to use that the program never need be concerned with whether a CPU is available.

## How the process solves

The basic concept: Virtualing the CPU

The basic technique: Time sharing of the CPU

The low-level machinery **mechanism**: **context switch**

The high-level **policies**: **scheduling policy**

#### The Abstraction

A **process** is the abstraction provided by the OS of a running program.

The components of machine state:

* memory: the instructions and the data sit in memory
* register: such as **program counter**(**instruction pointer**), **stack point**, **frame point**

Process API:

* Create
* Destroy
* Wait
* Miscellaneous Control
* Status

#### context switch

#### scheduling policy


##### 线程解决了什么问题

##### 线程是怎样解决的（线程是什么）



