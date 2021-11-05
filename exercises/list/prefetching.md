# Prefetching

Consider a data cache with a block size of 16 bytes. The bus that connects the data cache to the
memory is 8 bytes wide, i.e., per clock cycle, 8 bytes can be transferred between the data cache
and main memory. Assume that the access time to main memory equals 5 cycles; this is the time
between the request to the data cache block and the appearance of the first word on the bus. As
a result, fetching a full data cache block from main memory takes 7 cycles. The data cache has
one read/write port.
Assume an in-order processor with a width of 1. The execution of all instructions takes one
cycle. Assume a blocking cache, i.e., when a cache miss occurs, no new memory operation can
be executed.
Consider the following piece of code for a 64-bit architecture (assume that the content of r3 and
r4 is initially zero):
```
       li r2,1000    ; write the immediate value 1000 in r2
loop:  ldai r5,A(r3) ; load and increment pointer in r3
                     ; read in array A
       ldai r6,B(r4) ; read in array B
       add r7,r7,r5
       add r7,r7,r6
       bdnz r2,loop  ; decrement r2, branch if not zero
```
(Note that the ldai instruction increments the pointer with the value 8.)

(a) Indicate the flow of instructions in the following table. Mark a data cache miss with ‘m’;
the transfer with ‘t’ and the instructions from the loop as ‘A’, ‘B’, ‘+’, ‘+’ and ‘br’.

|Cycle  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10| 11| 12| 13|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A| M | M | M | M | M | M | T | T |   |   |   |   |   |   |
|Array B|   |   |   |   |   |   |   |   |   | M | M | M | M | M |
|Instr  | A |   |   |   |   |   |   |   | B | + |   |   |   |   |

|Cycle  | 14| 15| 16| 17| 18| 19| 20| 21| 22| 23| 24| 25| 26| 27|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A|   |   |   |   |   |   |   |   |   |   | M | M | M | M |
|Array B| T | T |   |   |   |   |   |   |   |   |   |   |   |   |
|Instr  |   |   | + | Br| A | B | + | + | Br| A |   |   |   |   |

|Cycle  | 28| 29| 30| 31| 32| 33| 34| 35| 36| 37| 38| 39| 40| 41|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A| M | T | T |   |   |   |   |   |   |   |   |   |   |   |
|Array B|   |   |   |   | M | M | M | M | M | T | T |   |   |   |
|Instr  |   |   |   | B | + |   |   |   |   |   |   | + | Br| A |

(b) Now assume that multiple outstanding cache misses can be handled concurrently, i.e., we
assume a non-blocking cache. How is the flow of instructions now? We assume a so-called split-
transaction bus that assigns tags to bus transactions. This enables handling multiple transactions in
parallel.

|Cycle  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10| 11| 12| 13|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A|   | M | M | M | M | M | T | T |   |   |   |   |   |   |
|Array B|   | M | M | M | M | M | M |   | T | T |   |   |   |   |
|Instr  | A | B |   |   |   |   |   |   | + |   | + | Br| A |   |

|Cycle  | 14| 15| 16| 17| 18| 19| 20| 21| 22| 23| 24| 25| 26| 27|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A|   |   |   |   | M | M | M | M | M | T |T  |   |   |   |
|Array B|   |   |   |   |   | M | M | M | M | M |   | T | T |   |
|Instr  | + | + | Br| A | B |   |   |   |   |   |   | + |   | + |

|Cycle  | 28| 29| 30| 31| 32| 33| 34| 35| 36| 37| 38| 39| 40| 41|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A|   |   |   |   |   |   |   | M | M | M | M | M | T | T |
|Array B|   |   |   |   |   |   |   |   | M | M | M | M | M |   |
|Instr  | Br| A | B | + | + | Br| A | B |   |   |   |   |   |   |

(c) Now assume that prefetching is also implemented. We assume a stream buffer that fetches
the next cache block when a cache miss occurred. A normal load instruction obviously has
precedence to a prefetch operation. Indicate the flow of instructions in the following table.

|Cycle  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10| 11| 12| 13|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A|   | M | M | M | M | M | T | T | M | M | M | M | M | T |
|Array B|   | M | M | M | M | M | M |   | T | T | M | M | M | M |
|Instr  | A | B |   |   |   |   |   |   | + |   | + | Br| A | B|

|Cycle  | 14| 15| 16| 17| 18| 19| 20| 21| 22| 23| 24| 25| 26| 27|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A| T |   |   |   | M | M | M | M | M | T | T |   |   |   |
|Array B| M | T | T |   |   | M | M | M | M | M |   | T | T |   |
|Instr  | + | + | Br| A | B | + | + | Br| A | B | + | + | Br| A |

|Cycle  | 28| 29| 30| 31| 32| 33| 34| 35| 36| 37| 38| 39| 40| 41|
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Array A| M | M | M | M | M | M | T | T |   |   |   |   |   |   |
|Array B|   | M | M | M | M | M |   | T | T |   |   |   |   |   |
|Instr  | B | + | + | Br|   |   |   |   |   |   |   |   |   |   |
