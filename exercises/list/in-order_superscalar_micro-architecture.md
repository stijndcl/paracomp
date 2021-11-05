# In-order superscalar micro-architecture
Consider the following piece of code:
```
add r1,r2,r3
ld
$4(A),r4
mul r3,r4,r2
add r1,r4,r2
sub r2,r1,r5
ld
$8(A),r4
add r1,r4,r3
add r2,r4,r5
add r3,r2,r5
```

In this (fictitious) ISA, the target register is shown on the right. In other words,
add_r1,r2,r3 means that the contents of registers r1 and r2 are added and that the result 
is written in register r3.

Assume an in-order superscalar architecture with a pipeline width of two (i.e., each pipeline stage
can hold up to two instructions). Indicate in the following table the flow of instructions through
the different pipeline stages over time. Assume a simple pipeline similar to the one explained in
the lectures: IF—ID—OF—EX—WB. The execution of a load instruction takes three cycles
(MEM1—MEM2—MEM3) and the data is available only after MEM3. The execution of a
multiplication takes four cycles (EX1—EX2—EX3—EX4). The execution of all other
instructions takes one cycle. Assume further that there are enough functional units of all types,
but only there is one write port to the register file. Instructions that block due to dependences,
structural hazards, etc., block in the ID stage.

Also mark when forwarding is used—you can do this by drawing an arrow between the producer
and the consumer of a register value.

|              | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |16 |17 |18 |19 |20 |
|--------------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|add  r1,r2,r3 |IF |ID |OF |EX |WB |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
|ld   $4(A),r4 |IF |ID |OF |M1 |M1 |M3 |WB |   |   |   |   |   |   |   |   |   |   |   |   |   |
|mul  r3,r4,r2 |   |IF |ID |ID |ID |OF |EX1|EX2|EX3|EX4|WB |   |   |   |   |   |   |   |   |   |
|add  1,r4,r2  |   |IF |ID |ID |ID |ID |ID |ID |ID |OF |EX |WB |   |   |   |   |   |   |   |   |
|sub  r2,r1,r5 |   |   |IF |IF |IF |ID |ID |ID |ID |OF |M1 |M2 |M3 |WB |   |   |   |   |   |   |
|ld   $8(A),r4 |   |   |IF |IF |IF |IF |IF |IF |IF |ID |ID |ID |OF |EX |WB |   |   |   |   |   |
|add  r1,r4,r3 |   |   |   |   |   |IF |IF |IF |IF |IF |ID |ID |ID |OF |EX |WB |   |   |   |   |
|add  r2,r4,r5 |   |   |   |   |   |   |   |   |   |IF |ID |ID |ID |ID |OF |EX |WB |   |   |   |
|add  r3,r2,r5 |   |   |   |   |   |   |   |   |   |   |IF |ID |ID |ID |OF |EX |WB |  |  |  |
