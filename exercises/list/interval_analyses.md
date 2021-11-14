# Interval analysis

Consider a two-wide out-of-order processor with a non-blocking memory hierarchy and a
reorder buffer of 16 entries. The following four code fragments are executed on this processor:
 ``` 
(1)
ld A(R1) -> R1
brz R2, label
ld A(R1) -> R1
label:
...

(2)
ld A(R1) -> R1
brz R1, label
ld A(R1) -> R1
label:
...

(3)
ld A(R1) -> R1
brz R2, label
ld A(R2) -> R2
label:
...

(4)
ld A(R1) -> R1
brz R1, label
ld A(R2) -> R2
label:
...

```

Assume that the two load operations in all four fragments cause a miss in the last-level cache, and
that the branch is not taken, but it is predicted taken by the branch predictor. The latency of a
last-level cache miss is much longer than the penalty of a branch misprediction (e.g., 200 cycles
for the cache miss versus 10 cycles for the branch misprediction). The total penalty encountered
by each fragment can be one of the following possibilities:

a) The sum of the penalties of both cache misses and the branch misprediction, i.e., the
penalties of all misses serialize (a total penalty of 410 cycles in the example).

b) The sum of the penalties of the two cache misses, i.e., the penalty of the branch
misprediction is hidden (a total penalty of 400 cycles in the example).

c) The sum of the penalties of one cache miss and the branch misprediction, i.e., the penalty
of one cache miss is hidden (a total penalty of 210 cycles in the example).

d) The penalty of one cache miss, i.e., the penalties of the other cache miss and the branch
misprediction are both hidden (a total penalty of 200 cycles in the example).

Which penalty is encountered by which fragment (e.g., fragment 1 has penalty b, fragment 2 has
penalty d, etc.)? Justify your answers with interval analysis diagrams, showing the number of
instructions dispatched as a function of time for each fragment.

_Drawings will be placed on Ufora_

Fragment (1): **B**

Fragment (2): **A**

Fragment (3): **C**

Fragment (4): **A**
