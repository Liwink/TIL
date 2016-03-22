## Mechanism: Address Translation

### Limited direct execution

In developing the virtualization of the **CPU**: 

* the program run directly on the hardware for the most part (efficiency)
* the OS interpose at critical points (control)

**hardware-based address translation**:

* the hardware transforms each memoty access, changing the **virtual** address provided by the instruction to a **physical** address.
* the way that the hardware **relocate** this process in memory is **transparent** to the process

### Dynamic (Hardware-based) Relocation

**base and bounds**:

* a **base** register is used to transform virtual addresses into physical addresses
* a **bounds** register ensures that such addresses are within the confines of the address space 

### Hardware Support: A Summary

| **Hardwar Requirements** | **Notes** |
| --- | ------ |
| Privileged mode | Needed to prevent **user-mode processes** from executing privileged operations |
| Bsae/bounds registers | Need pair of registers per CPU to support address translation and bounds checks|
| Translate virtual addresses and check if within bounds| Circuitry to do translations and check limits |
| Privileged instructions(s) to registerexception handlers | OS must be able to tell hardware what code to run if exception occurs |
| Ability to raise exceptions | When processes try to access privileged instructions or out-of-bounds memory |

### Operating System Issues

1. finding space for process's address space in memory when it is created
2. reclaiming all of process's memory when it is terminated
3. saving and restoring the base-and-bounds pairs when it switches between processes (only one base and bounds register pair on each CPU)
4. providing exception handlers to take action when the CPU raise an exception






