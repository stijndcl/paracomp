# Hybrid tournament branch predictor
Consider a hybrid tournament branch predictor consisting of
- a bimodal predictor (2 elements – PHT0 and PHT1 – each having a 2-bit saturating
counter)
- a gshare predictor (2 elements, each having a 2-bit saturating counter and 1-bit BHR)
- a metapredictor (2 elements). The metapredictor is indexed alike the bimodal predictor.
The metapredictor selects the outcome of the bimodal predictor if the saturating 2-bit
counter is strictly smaller than 2 and selects the outcome of the gshare predictor if the
counter is larger than or equal to 2.

Complete the following table. Note that all instructions have a size of 4 bytes (RISC ISA); in
other words, the last two bits of the instruction address are always zero, and are therefore >
used for indexing the predictor.

| _________________________________________________________ | ______ state predictor AFTER executing the branch ______ |
| --- | --- |

| ______________________________ | ________ prediction _______  | metapredictor | __ bimodal __ | ______ gshare ______ |
| ---                       | ---                           | ---           | ---     |---      |

| branch address  | branch direction  | bimodal | gshare  | hybrid  | PHT0  | PHT1  | PHT0  | PHT1  | BHR | PHT0  | PHT1  |
| ---             | ---               | ---     | ---     | ---     | ---   | ---   | ---   | ---   | --- | ---   | ---   |
|                 |                   |         |         |         | 2     | 0     | 0     | **2** |**0**| **2** | **1** |
| 0x654           | N                 | **T**   | **N**   | **T**   | **2** | **1** | **0** | **1** |**0**| **2** | **0** |
| 0x780           | T                 | **N**   | **T**   | **T**   | **3** | **1** | **1** | **1** |**1**| **3** | **0** |
| 0x654           | T                 | **N**   | **T**   | **N**   | **3** | **2** | **1** | **2** |**1**| **3** | **0** |
| 0x780           | T                 | **N**   | **N**   | **N**   | **3** | **2** | **2** | **2** |**1**| **3** | **1** |
| 0x654           | T                 | **T**   | **T**   | **T**   | **3** | **2** | **2** | **3** |**1**| **3** | **1** |
| 0x780           | N                 | **T**   | **N**   | **N**   | **3** | **2** | **1** | **3** |**0**| **3** | **0** |

### Notes

**Branch direction** \
The direction the branch went in in reality.

**PHT0 or PHT1** \
0x654 -> to binairy -> ...0100 -> PHT1 \
0x780 -> to binairy -> ...0000 -> PHT0 \
So for address 0x654 we only look at PHT1 (and possibly BHR) \
and for address 0x780 we only look at PHT0 (and possibly BHR)

**Bimodal predictor** \
Predicting: Taken if >= 2 | Not Taken if < 2 \
Updating: + 1 if Taken | - 1 if Not Taken

**Gshare predictor** \
For the BHR:\
First you do BHR XOR addr and then you pick PHT0 or PHT1 \
Updating: 0 if previous branching was Not Taken | 1 if previous branching was Taken \
Then for the PHT's: \
Predicting: Taken if >= 2 | Not Taken if < 2 \
Updating: + 1 if Taken | - 1 if Not Taken
