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
|                 |                   |         |         |         | 2     | 0     | 0     | 2     | 0   | 2     | 1     |
| 0x654           | N                 |
| 0x780           | T                 |
| 0x654           | T                 |
| 0x780           | T                 |
| 0x654           | T                 |
| 0x780           | N                 |
