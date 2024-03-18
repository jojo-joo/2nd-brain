## Background
old stuff wrote in 2003, not very perfect, but it's also a small summary of the past, mainly focusing on the x86 platform, collected as follows:

## Short to int
### Description
In 32-bit programs, using the int type as the index variable in a loop performs better than using the short type. The loop index variable refers to i in the loop for (i=0; i<9; i++).

### Background
Using short in 32-bit applications leads to longer machine code, and the processor requires more time to process it. Furthermore, most compilers, when not optimized, always convert short to integer before performing various operations and then convert it back to short after the operations, adding unnecessary overhead.

### Original Code
``` c
unsigned short i;

for (i=0; i<N; i++)
{
    DoSomething();
}
```
### Optimized Code
```
unsigned int j;

for (j=0; j<N; j++)
{
    DoSomething();
}
```
## Pointers to register
### Description
Try to minimize the use of pointer indirect addressing in loops. Some indirect addressing can be avoided by first storing the contents of the indirectly addressed memory in a register if possible. After operations within the loop, the data can then be written back to the memory pointed to by the pointer.

### Background
Indirect addressing leads to at least two memory operations. Additionally, it introduces dependencies between instructions, as the address must be obtained before accessing memory contents. This reduces the compiler's optimization capabilities and can also impact the processor's pipeline, including out-of-order execution, scheduling, and instruction pairing.

### Original Code

```c
int i, j = 0, *a = &j;

for (i=0; i<N; i++)
{
    *a += i;
}
```
### Optimized Code

```c
int i, j = 0, *a = &j;
register int temp;

temp = *a;
for (i=0; i<N; i++)
{
    temp += i;
}
*a = temp;
```
## Move if statement out of loop core
### Description
If there is a logical condition inside the loop body and the loop iterates many times, it is advisable to move the logical condition outside the loop body.

### Background
In the original code below, the logical condition is executed N-1 times, and the loop disrupts the original sequence of instructions in the processor, reducing efficiency.

### Original Code

```c
for (i=0; i<N; i++)
{
    if (condition)
        DoSomething();
    else
        DoOtherthing();
}
```
### Optimized Code

```c
if (condition)
{
    for (i=0; i<N; i++)
        DoSomething();
}
else
{
    for (i=0; i<N; i++)
        DoOtherthing();
}
```

## Use library
### Description
If the functionality you need can be found in a library function, use it, especially if you're not an expert.

### Background
Library functions are generally optimized for your operating system, and their performance is incomparable to typical code. For example, in the code below, the performance of the same memory copy operation could be up to ten times better.

### Original Code

```c
unsigned char output[N], input[N];

for (i = 0; i < N; i++)
{
    output[i] = input[i];
}
```
### Optimized Code:

```c
unsigned char output[N], input[N];

memcpy(output, input, N);
```
> **Performance Comparison**
>
> Performance will be significantly improved.

## Division to multiplication
### Description
Converting a floating-point division operation inside a loop to a floating-point multiplication may yield unexpected results.

### Background
Floating-point division is much slower than multiplication. Although integer multiplication and division have no difference in performance, bitwise operations can be used to improve efficiency in appropriate situations. For instance, shifting operations can be utilized to improve efficiency. Linux kernel has made significant improvements in division and modulo operations.

### Original Code

```c
int i;
double j;

for (i=0; i<0xffffff; i++)
{
    j = (double)i / 3.3;
}
```
### Optimized Code

```c
int i;
double j, k;

k = (double)1.0 / 3.3;
for (i=0; i<0xffffff; i++)
{
    j = (double)i * k;
}
```
> **Performance Comparison**
>
> Local speed increases by approximately 85%.

## For statement
### Description
In nested loops, if possible, place the longest loop inside and the shortest loop outside to minimize CPU's traversal through loop layers.

### Background
Floating-point division is much slower than multiplication. Although integer multiplication and division have no difference in performance, bitwise operations can be used to improve efficiency.

### Original Code
``` c
int i;
double j;

for (i=0; i<0xffffff; i++)
{
    j = (double)i / 3.3;
}
```

### Optimized Code
``` c
int i;
double j, k;

k = (double)1.0 / 3.3;
for (i=0; i<0xffffff; i++)
{
    j = (double)i * k;
}
```
> **Performance Comparison**
>
> Local speed increases by approximately 85%.

## Reference: 
- "High-Quality C++/C Coding Standards"
- 'C' Coding Techniques for Intel Architecture Processors