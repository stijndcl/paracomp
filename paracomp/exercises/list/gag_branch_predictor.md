# GAg branch predictor
(a) Consider a GAg branch predictor. There are three history bits. What is the content of the
branch predictor table after a (sufficiently long) execution of the following sequence of branch
outcomes: TTTNNTTTNNTTTNN... where T stands for taken and N stands for not taken.
Assume 2-bit saturating counters in the PHT.

(b) Now consider a GAg branch predictor with two history bits, and the same branch outcome
sequence: TTTNNTTTNNTTTNN... What is the accuracy of this branch predictor during
steady-state (i.e., after the initialization phase) assuming all 2-bit saturating counters have an initial
value of 1?

(c) Consider the following control flow graph:

![control flow graph](gag_branch_predictor_img1.png) 

The fat curve indicates how this control flow graph is traversed during the execution of the
program. Write down the branch behavior of the four conditional branches (denoted in gray in
the figure): use T for taken and N for not-taken.

Branch behavior for branch (a): \
Branch behavior for branch (b): \
Branch behavior for branch (c): \
Branch behavior for branch (d): 

How many history bits does a PAg predictor need to perfectly predict the behavior of these four
branches (after initialization)? (Assume there is no aliasing in the first level of the PAg branch
predictor.)
