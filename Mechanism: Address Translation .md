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




