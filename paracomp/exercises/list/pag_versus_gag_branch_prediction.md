# PAg versus GAg Branch Prediction

Consider the below sequence of branch outcomes (Taken versus Not-taken) for branches A, B,
C, D and E. Note that the sequence ABEACEACDE is repeatedly executed.

|A|B|E|A|C|E|A|C|D|E|A|B|E|A|C|E|A|C|D|E|A|B|...|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|---|
|T|T|N|N|T|N|N|N|N|N|T|T|N|N|T|N|N|N|N|N|T|T|   |

```
A: TNNTNN
B: TTTT
C: TNTNTN
D: NNN
E: NNN
```

We compare a PAg versus a GAg branch predictor under steady-state conditions, i.e., after
executing the above branch sequence for a long enough time so that initialization effects have
passed. The address bits used to index the PAg predictor are chosen such that there is no
aliasing in its Branch History Table. We further assume two-bit saturating counters in the
Pattern History Tables for both the PAg and GAg predictors.

(a) What is the minimum size (in bits) for the PAg branch predictor to achieve perfect
accuracy (no mispredictions) during steady-state?

(b) What is the minimum size (in bits) for the GAg branch predictor to achieve perfect
accuracy (no mispredictions) during steady-state?
