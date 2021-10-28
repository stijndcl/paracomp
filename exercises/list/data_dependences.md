# Data dependences

Consider the following piece of code:
```
add r1,r2,r3
ldi $256,r4
mul r3,r4,r2
sub r2,r1,r5
ld
$8(A),r4
add r1,r4,r3
ld
$24(A),r4
mul r4,r3,r5
```

In this (fictitious) ISA, the target register is shown on the right. In other words,
add r1,r2,r3 means that the contents of registers r1 and r2 are added, and that the result
is written into register r3. The ldi (load immediate) instruction means that a constant value is
written into the target register.

(a) Indicate all true data dependences.
```
add r1,r2,r3
ldi $256,r4
mul r3,r4,r2
sub r2,r1,r5
ld $8(A),r4
add r1,r4,r3
ld $24(A),r4
mul r4,r3,r5
```

(b) Indicate all anti dependences.
```
add r1,r2,r3
ldi $256,r4
mul r3,r4,r2
sub r2,r1,r5
ld $8(A),r4
add r1,r4,r3
ld $24(A),r4
mul r4,r3,r5
```

(c) Indicate all output dependences.
```
add r1,r2,r3
ldi $256,r4
mul r3,r4,r2
sub r2,r1,r5
ld $8(A),r4
add r1,r4,r3
ld $24(A),r4
mul r4,r3,r5
```

(d) Apply register renaming. Assume there is a sufficient number of physical registers.

(e) Suppose that the execution of a load instruction takes 2 cycles; an addition, subtraction and
load-immediate takes 1 cycle; and a multiplication takes 4 cycles. Determine the minimum
number of execution cycles using the data flow graph.

(f) Now suppose that the first load instruction (ld $8(A),r4) causes a cache miss in the L1
D-cache, but a hit in the L2 cache. The access time to the L2 cache is 10 cycles. What is the
execution time now?

Number of cycles:

(g) Suppose again that the first load instruction is a cache hit. Assume further that addition,
subtraction and load-immediate instructions are executed on a functional unit of type A; that a
multiplication is executed on a functional unit of type B; and that a load instruction is executed
on a functional unit of type C. All function units are fully pipelined. Determine the minimum
number of functional units of each type, such that the total execution time of this piece of code
does not increase, and thus remains the same as in (e). Indicate when instructions are executed in
the following table.
|       | 1| 2| 3| 4|
|-------|--|--|--|--|
|Cycle 1 |  |  |  |  |
|Cycle 2 |  |  |  |  |
|Cycle 3 |  |  |  |  |
|Cycle 4 |  |  |  |  |
|Cycle 5 |  |  |  |  |
|Cycle 6 |  |  |  |  |
|Cycle 7 |  |  |  |  |
|Cycle 8 |  |  |  |  |
|Cycle 9 |  |  |  |  |
|Cycle 10|  |  |  |  |

How many functional units of type A are required? \
unit(s)

How many functional units of type B are required? \
unit(s)

How many functional units of type C are required? \
unit(s)
