# Register Renaming and Load Bypassing

Consider the below x86-64 code excerpt from the SPEC CPU hmmer benchmark.
```
mov rdx, qword ptr [rsp+0x58]
mov eax, dword ptr [rdx+rcx*1]
add eax, dword ptr [rbp+rcx*1]
mov dword ptr [rsi+rcx*1+0x4], eax
mov r9, qword ptr [rsp+0x70]
mov edx, dword ptr [r9+rcx*1]
add edx, dword ptr [rbx+rcx*1]
cmp edx, eax
cmovl edx, eax
mov dword ptr [rsi+rcx*1+0x4], edx
```
See earlier descriptions regarding the x86 ISA. (Note that rdx, rsp, eax, rcx, rsi, r9 are
architectural registers.) The compare instruction (cmp) compares two register arguments and
sets a flag, which the conditional-move instruction (cmovl) reads. The cmovl instruction
copies (moves) the content of register eax to register edx if the comparison is ‘less than’.

(a) Apply register renaming to the above code excerpt.

zie foto

(b) Consider an out-of-order processor with dynamic memory disambiguation. Does load
bypassing improve performance for the above code? If so, under which specific conditions?
Be precise.

