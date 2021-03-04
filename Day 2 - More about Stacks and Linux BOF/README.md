### Introduction to the Stack

This is the area of memory within a process which is used by processes itself to save data. Contrary to registers which are small in size, the stack offers us a lot of space. This also helps to track the execution of the program.

When a new function is called to know what place the execution stopped the last address before function call is stored on the stack.

Which is the return address, as this where the program has to return. Stack is 4-byte aligned (32 bit) and it grows towards lower addresses. The stack starts with a high address and grows down to low memory addresses as values are added, the Base Pointer points to the beginning (base) of the stack in contrast to the Stack Pointer, which points to the top of the stack.

If no protections are in place we just operate on these Stacks as a normal piece in memory, and if we can overwrite them due to programmatical errors we can cause a overflow.
This mostly happens in C/C++ where you have predefine the size of the variable many time's.

We can use gdb to analyze these attacks and the stack and the values of the registers when such attacks happen and disassemble functions to get an idea of what they are doing etc.

```bash
gdb -q ./files
disas main # to analyze the main functions and see which other functions are called and stuff and what they are doing
```
Basically what we are trying to first find vulnerable points and then send a lot of characters to overwrite the EIP(which is what controls the execution flow of the program) and if we can do that then we are good to go.

And to get exact values of EIP we can use pattern_create and pattern_offset to create and get the values of these fields.

```bash
gdb-peda$ pattern_create 1000 # Creates a cyclic pattern and then you can use pattern_offset to basically check the crash
gdb-peda$ pattern_offset 0x4e734138 # This tells us to the offset
```

Also some necessary things to check before hand :

Check what Endian is being used and to this we can do readelf or file on the binary we have and we will see if its LSB (Least Significant Bit, Little Endian) or MSB (Most Significant Bit, big Endian)
and then check for security on the binary and what we have permissions to do like is NX enabled which if enabled we cannot use shellcode and execute from the stacks and what stuff would we have to bypass like ASLR and stuff.
