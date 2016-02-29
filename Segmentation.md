## Segmentation

##### THE CRUX: HOW TO SUPPORT A LARGE ADDRESS SPACE

How dox we support a large address space with (potentially) a lot of free space between the stack and the heap?

#### Segmentation: Generalized Base/Bounds

Why not have a base and bounds pair per logical segment of the address space?

What segmentation allows the OS to do is to place each one of those segments in different parts of physical memory, and thus avoid filling physical memoty with unused vitual address space.

for example:

| Segment | Base | Size | Grows Positive? | Protection |
| ------- | ---- | ---- | --------------- | ---------- |
| Code    | 32K  | 2K   |        1        |  Read-Exec |
| Heap    | 34K  | 2K   |        0        |  Read-Write|
| Stack   | 28K  | 2K   |        0        |  Read-Write|

#### Which Segment Are We Referring To

* explicit: to chop up the address space into segments based on the top fews bits of the virtual address
* implicit: the hardware determines the segment by noticing how the address was formed 

#### What About The Stack

*grows backwards*

#### Support for Sharing

To save memory, **code sharing** is still common in system today. 
To support sharing, we need a little extra support from the hardware, in the form of **protection bits**.

#### Fine-grained vs. Coarse-grained Segmentation

#### OS Support

