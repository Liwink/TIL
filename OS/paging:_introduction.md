## Paging: Introduction

When dividing a space into *different-size* chunks, the space itself can become *fragmented*, and thus allocation becomes more challenging over time.

Thus, it is worth considering the second approach: to chop up space into *fixed-sized* pieces.

In *virtual memory*, we call this idea **paging**. Correspondingly, we view *physical memory* as an array of fixed-sized slots called **page frames**.

> How can we virtualize memory with pages, so as to avoid the problems of segmentation? What are the basic techniques? How do we make those techniques work well, with minimal space and time overheads?


### A Simple Example And Overview

* **page table**: *per-process* data structure, **address translations** (VP3->PF2)
* 