### Introduction to Assembly x86-64

Resource Used : https://tryhackme.com/room/introtox8664
Since the architecture is x86-64, the registers are 64 bit and Intel has a list of 16 registers:

In 64bit architecture the registers start from r and in the 32 bit the registers start from 32 bit refer to the THM room for the registers list.

They can hold upto 64 bits of data, other parts of the register can also be referenced.registers can also be referenced as 32 bit values as shownThe %rsp is the stack pointer and it points to the top of the stack which contains the most recent memory address. The stack is a data structure that manages memory for programs.

-   `leaq source, destination`: this instruction sets destination to the address denoted by the expression in source
    
-   `addq source, destination`: destination = destination + source
    
-   `subq source, destination`: destination = destination - source
    
-   `imulq _source, destination_`: destination = destination \* source
    
-   `salq source, destination`: destination = destination << source where << is the left bit shifting operator
    
-   `sarq source, destination`: destination = destination >> source where >> is the right bit shifting operator
    
-   `xorq source, destination`: destination = destination XOR source
    
-   `andq source, destination`: destination = destination & source
    
-   `orq source, destination`: destination = destination | source


If statements use 3 important instructions in assembly:

-   `cmpq source2, source1`_:_ it is like computing a-b without setting destination
    
-   `testq source2, source1`: it is like computing a&b without setting destination

jmp: unconditional 
je : equal/zero
jne : not equal/ not zero
js : negative
jns : nonnegative
jg : greater
jge : greate or equal 
jl;: less 
jle : less or equal
ja : above (unsigned)
jb : below (unsigned)

--underwork---