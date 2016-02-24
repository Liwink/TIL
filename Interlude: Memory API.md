## Interlude: Memory API

### Types of Memory

* **stack**: *implicitly*, *automatic* memory, `int x;`
* **heap**: *explicitly*, `int *x = (int *) malloc(sizeof(int))`

### The malloc() Call

The `malloc()`: you pass it a size asking for some room on the *heap*, and it either succeeds and gives you back a *pointer* to the newly-allocated space, or fails and returns *NULL*.

### The free() Call

```
int *x = malloc(10 * sizeof(int));

free(x);
```

### Common Errors

> Correct memory management has been such a prblem, in fact, that many newer languages has support for **automatic memory management**. In such languages, while you call somethine akin to malloc() to allocate memory (usually `new`), you never have to call something to free space; rather, a **garbage collector** runs figures out what memory you no longer have references to and frees it for you.

* Forgetting To Allocate Memory
* Not Allocating Enough Memory
* Forgetting to Initialize Allocated Memory
* Forgetting To Free Memory (**memory leak**)
* Freeing Memory Befor You Are Done With It
* Freeing Memory Repeatedly
* Calliing free() Incorrectly




---

* compile time and runtime
* heap and stack
* garbage collector

##### When to Use Heap

> If you need to allocate a large block of memory (e.g. a large array, or a big struct), and you need to keep that variable around a long time (like a global), then you should allocate it on the heap.