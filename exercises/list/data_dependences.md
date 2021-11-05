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
| # | instr | writes to | |
| - | ----- | - |-|
|1|add r1,r2,r3  | r3 | |
|2|ldi $256,r4   | r4 | |
|3|mul r3,r4,r2  | r2 | (r3 from 1) (r4 from 2)|
|4|sub r2,r1,r5  |    | (r2 from 3)|
|5|ld $8(A),r4   | r4 | |
|6|add r1,r4,r3  | r3 | (r4 from 5)|
|7|ld $24(A),r4  | r4 | |
|8|8mul r4,r3,r5 |    | (r4 from 7) (r3 from 6)|

(b) Indicate all anti dependences.
| # | instr | _ | |
| - | ----- | - |-|
|1|add r1,r2,r3  | r2     | |
|2|ldi $256,r4   |        | |
|3|mul r3,r4,r2  | r4, r3 | (from 1)|
|4|sub r2,r1,r5  |        | |
|5|ld $8(A),r4   |        | (from 3)|
|6|add r1,r4,r3  | r4     | (from 3)|
|7|ld $24(A),r4  |        | (from 6)|
|8|8mul r4,r3,r5 |        | |

(c) Indicate all output dependences.
| # | instr | _ | |
| - | ----- | - |-|
|1|add r1,r2,r3  | r3 | |
|2|ldi $256,r4   | r4 | |
|3|mul r3,r4,r2  |    | |
|4|sub r2,r1,r5  | r5 | |
|5|ld $8(A),r4   | r4 | (from 2)|
|6|add r1,r4,r3  |    | (from 1)|
|7|ld $24(A),r4  |    | (from 5)|
|8|8mul r4,r3,r5 |    | (from 4)|

(d) Apply register renaming. Assume there is a sufficient number of physical registers.

|Arch Reg|Physical Reg     |add r1, r2 -> r3 | add F1, F2, -> F6  |
|--------|-----------------|-----------------|--------------------|
|R1      |F1               |ldi $256 -> r4   | ldi $256 -> F7     |
|R2      |F2, F8           |mul r3, r4 -> r2 | mul F6, F7 -> F8   |
|R3      |F3, F6, F11      |sub r2, r1 -> r5 | sub F8, F1 -> F9   |
|R4      |F4, F7, F10, F12 |ld $8(A) -> r4   | ld $8(A) -> F10    |
|R5      |F5, F9, F13      |add r1, r4 -> r3 | add F1, F10 -> F11 |
|        |                 |ld $24(A) -> r4  | ld $24(A) -> F12   |
|        |                 |mul r4, r3 -> r5 | mul F12, F11 -> F13|

(e) Suppose that the execution of a load instruction takes 2 cycles; an addition, subtraction and
load-immediate takes 1 cycle; and a multiplication takes 4 cycles. Determine the minimum
number of execution cycles using the data flow graph.

Add F1, F2 -> F6 & ldi $256 -> F7  🡪 mul F6, F7 -> F8 🡪 sub F8, F1 -> F9               6 cycles
ld $8(A) -> F10 🡪 add F1, F10 -> F11 & ld $24(A) -> F12 🡪 mul F12, F11 -> F13   7 cycles
In total 7 cycles:  add = 1, ldi = 1, mul = 4, sub = 1, ld = 2


(f) Now suppose that the first load instruction (ld $8(A),r4) causes a cache miss in the L1
D-cache, but a hit in the L2 cache. The access time to the L2 cache is 10 cycles. What is the
execution time now?

 17 cycles: 2 cycles voor de miss te zien en dan nog eens 10 cycles voor de L2 te accessen 

Number of cycles: **17**

(g) Suppose again that the first load instruction is a cache hit. Assume further that addition,
subtraction and load-immediate instructions are executed on a functional unit of type A; that a
multiplication is executed on a functional unit of type B; and that a load instruction is executed
on a functional unit of type C. All function units are fully pipelined. Determine the minimum
number of functional units of each type, such that the total execution time of this piece of code
does not increase, and thus remains the same as in (e). Indicate when instructions are executed in
the following table.
|       | 1| 2| 3| 4|
|-------|--|--|--|--|
|Cycle 1 | add F1, F2 -> F6 |  | ld $8(A) |  |
|Cycle 2 | ldi $256 -> F7 |  | ld $24(A) -> F12 |  |
|Cycle 3 | add F1, F10 -> F11| MUL F6, F7 -> F8 |  |  |
|Cycle 4 |  | Mul F12, F11 -> F13 |  |  |
|Cycle 5 |  |  |  |  |
|Cycle 6 |  |  |  |  |
|Cycle 7 | Sub F8, F1 -> F9 |  |  |  |

How many functional units of type A are required? \
1 unit(s)

How many functional units of type B are required? \
1 unit(s)

How many functional units of type C are required? \
1 unit(s)
