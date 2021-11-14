<!-- @format -->

## p47 Cache misses

a) Indicate the access pattern for both implementations for an array consisting of 16 elements and a cache block size of 16 bytes. Mark the element that is referenced first with a '1', the second element with a '2', etc. Use circles to mark the memory references that will be cache misses.

_Note: I indicate cache misses in **bold** instead of using circles because Markdown_

Implementation 1:

[**1**, 2, 3, 4, **5**, 6, 7, 8, **9**, 10, 11, 12, **13**, 14, 15, 16]

Implementation 2:

_This one doesn't fill them from left to right but it jumps all over the place because of the loop order_

[**1**, 5, 9, 13, **2**, 6, 10, 14, **3**, 7, 11, 15, **4**, 8, 12, 16]

b) Can there be a difference in execution speed for both implementations?

Yes, the second one will be faster:

- The first implementation will load a cache block, but all elements are in the same cache line so the next cache block isn't loaded as it isn't needed yet
- The second will load all 4 cache blocks after each other because all of them are required from the start

## p48 Interval Analysis

_Drawings will be placed on "The Ufora" so I will not make fancy stuff in Inkscape_

1 - B
2 - A
3 - C
4 - A

## p50 Iron Law of Performance

**A has the best performance**

Calculate CPI:

<!-- $$CPI_A = 2 \times 0.4 + 3 \times 0.25 + 3 \times 0.25 + 7 \times 0.1 = 3$$ -->
<img src="https://render.githubusercontent.com/render/math?math=CPI_A%20%3D%202%20%5Ctimes%200.4%20%2B%203%20%5Ctimes%200.25%20%2B%203%20%5Ctimes%200.25%20%2B%207%20%5Ctimes%200.1%20%3D%203">

<!-- $$CPI_B = 2 \times 0.15 + 2 \times 0.15 + 4 \times 0.10 + 6 \times 0.10 + 1 \times 0.1 + 2 \times 0.2 + 2 \times 0.2 = 2.5$$ -->
<img src="https://render.githubusercontent.com/render/math?math=CPI_B%20%3D%202%20%5Ctimes%200.15%20%2B%202%20%5Ctimes%200.15%20%2B%204%20%5Ctimes%200.10%20%2B%206%20%5Ctimes%200.10%20%2B%201%20%5Ctimes%200.1%20%2B%202%20%5Ctimes%200.2%20%2B%202%20%5Ctimes%200.2%20%3D%202.5">

Calculate time:

No need to know or calculate N because it cancels out in the fraction anyways

<!-- $$T = CPI \times N \times \frac{1}{frequency}$$ -->
<img src="https://render.githubusercontent.com/render/math?math=T%20%3D%20CPI%20%5Ctimes%20N%20%5Ctimes%20%5Cfrac%7B1%7D%7Bfrequency%7D">

<!-- $$T_A = 3 \times N \times \frac{1}{600 mHz}$$ -->
<img src="https://render.githubusercontent.com/render/math?math=T_A%20%3D%203%20%5Ctimes%20N%20%5Ctimes%20%5Cfrac%7B1%7D%7B600%20mHz%7D">

<!-- $$T_B = 2.5 \times 1.5 N \times \frac{1}{700 mHz}$$ -->
<img src="https://render.githubusercontent.com/render/math?math=T_B%20%3D%202.5%20%5Ctimes%201.5%20N%20%5Ctimes%20%5Cfrac%7B1%7D%7B700%20mHz%7D">

<!-- $$\frac{T_A}{T_B} = \frac{21}{22.5}$$ -->
<img src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7BT_A%7D%7BT_B%7D%20%3D%20%5Cfrac%7B21%7D%7B22.5%7D">

<!-- $$T_A \lt T_B \rightarrow P_A > P_B$$ -->
<img src="https://render.githubusercontent.com/render/math?math=T_A%20%5Clt%20T_B%20%5Crightarrow%20P_A%20%3E%20P_B">

## p51 Performance

_**NOT ENOUGH INFORMATION TO TELL**_

We don't know the total number of instructions.

We don't know how much an instruction is doing, we only know that the ISA's are _"different"_ so we can't say anything about which will be faster.

How do both of them relate to each other.

## p53 Memory-Level Parallellism

a) How much speedup can the computer architect at most obtain by doubling the amount of memory-level parallellism (MLP)?

We're only looking at data misses, not instruction misses because if we have instruction misses we can't do much of anything.

Memory is 50% of execution time, so we have to halve that part (becomes 25).

<!-- $$\frac{100}{100 - (\frac{50}{2})} = \frac{100}{75} = 1.3333...$$ -->
<img src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7B100%7D%7B100%20-%20(%5Cfrac%7B50%7D%7B2%7D)%7D%20%3D%20%5Cfrac%7B100%7D%7B75%7D%20%3D%201.3333...">

b) Which of the below design changes should the computer arfchitect consider for increasing the amount of MLP? Fill in the below table.

| _**Design change**_                                         |                                                                                                                                              _**Yes or No?**_ |
| :---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| Make the pipeline deeper                                    |                                                                                                                                                            No |
| Increase superscalar width                                  |                                                                                                                    Yes, but makes everything more complicated |
| Increase ROB size                                           |                                                                                                               Yes, getting more data in parallell from memory |
| Increase size of reservation station                        | Generally no, but in some situations it might help. If we can _store_ more instructions in the RS it doesn't mean we can _execute_ more instructions as well. |
| Add a hardware prefetcher                                   |                                                                                                                                                           Yes |
| Increase the number of MSHRs                                |                                                                        Yes, if you have enough instructions to main memory and previous MSHR couldn't keep up |
| Increase L2 cache size                                      |                                                                                                      No, it's just more data not more instructions _together_ |
| Devise an improved cache replacement policy in the L2 cache |                                                                                                                                Yes, if the policy allows more |

c) depends how you look at it

- No, more correct load instructions, but the total number stays the same
- Yes, if you only look at the _useful_ MLP

## p52
