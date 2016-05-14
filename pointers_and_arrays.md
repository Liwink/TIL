## Pointers and Arrays

> A pointer is a variable that contains the **address** of a variable.
> Pointers are much used in C, partly because they are sometimes the only way to express a computation, and partly because they usually lead to more compact and efficient code than can be obtained in other ways.


##### Pointers and Addresses

```
int x = 1, y = 2, z[10];
int *ip;	// ip is a pointer to int

ip = &x;	// ip now points to x
y = *ip;	// y is now 1
*ip = 0;	// x is now 0
ip = &z[0]	// ip now points to z[0]
```

* `&` gives the address of an object
* `&` only applies to object in memory: variables and array elements.
* `&` cannot be applied to expressions, constants, or register variables (&constants?)
* every pointer points to a specific data type. (There is one exception: a "pointer to void" is used to hold * any type of pointer but connot be dereferenced itself.)

```
int *ap;
const int a = 0;
ap = &a; // &a???
```


##### Pointers and Function Arguments

C passes arguments to function by value.

##### Pointers and Arrays

```
int a[10];
int *pa;

pa = a;		// pa = &a[0]
*(pa+1); 	// equal to `a[1]`

char s[]; 	// equal to `\char *s`
```

##### Character Pointers and Functions

> When you write `char *a = "This is a string"`, the location of "This is a string" is in the executable, and the location a points to, is in the executable. The data in the executable image is **read-only**.

> What you need to do is create that memory in a location that is not read only--on the heap, or in the stack frame. 


##### reference

[pointer.c](../snippet/c/pointer.c)
