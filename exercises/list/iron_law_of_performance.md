# Iron Law of Performance

Consider two processors A and B with different ISAs. Processor A runs at 600 MHz;
processor B runs at 700 MHz. Processor B executes 50% more instructions than processor A
for getting the same amount of work done. Consider the following dynamic instruction mix
and the average number of cycles each instruction class takes for processor A:

| Instruction class | No. cycles | Frequency of occurrence |
|-------------------|------------|-------------------------|
| A                 | 2          | 40%                     |
| B                 | 3          | 25%                     |
| C                 | 3          | 25%                     |
| D                 | 7          | 10%                     |

and processor B:

| Instruction class | No. cycles | Frequency of occurrence |
|-------------------|------------|-------------------------|
| A                 | 2          | 15%                     |
| B                 | 2          | 15%                     |
| C                 | 4          | 10%                     |
| D                 | 6          | 10%                     |
| E                 | 1          | 10%                     |
| F                 | 2          | 20%                     |
| G                 | 2          | 20%                     |

Which processor yields the highest performance? Show your work.

**A has the best performance**

Calculate CPI:

$$CPI_A = 2 \times 0.4 + 3 \times 0.25 + 3 \times 0.25 + 7 \times 0.1 = 3$$
$$CPI_B = 2 \times 0.15 + 2 \times 0.15 + 4 \times 0.10 + 6 \times 0.10 + 1 \times 0.1 + 2 \times 0.2 + 2 \times 0.2 = 2.5$$

Calculate time:

No need to know or calculate N because it cancels out in the fraction anyways

$$T = CPI \times N \times \frac{1}{frequency}$$

$$T_A = 3 \times N \times \frac{1}{600 mHz}$$

$$T_B = 2.5 \times 1.5 N \times \frac{1}{700 mHz}$$

$$\frac{T_A}{T_B} = \frac{21}{22.5}$$

$$T_A \lt T_B \rightarrow P_A > P_B$$
