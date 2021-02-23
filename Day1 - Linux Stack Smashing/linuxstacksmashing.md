###  GDB Introduction

Resources used :

INE : www.ine.com : Exploit Development Student Course  

GDB is a tool we use to view the created binary on the assembler lever.

We can load a binary into gdb like this :

```bash
gdb -q bow32
```

and then to disassemble the main function we can use the following command :

```bash
disassemble main
```

I personally prefer intel syntax to set it like that we can use :

```bash
echo 'set disassembly-flavor intel' > ~/.gdbinit
```

Registers are the main parts of CPU, almost all registers offer a small part of storage space where data is temporarily stored.

Registers will be divided into General registers, Control registers, and Segment registers. The most critical registers we need are the General registers.

In GDB we also have a lot of plugins to improve the visibility provided to us via the plugin and from my research I have GDB Peda is very popular one so I will be using that one.
There GitHub can be found here : https://github.com/longld/peda
And the installation guide is like so

```bash
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
```

We can also use GDB in a quiet mode to not print a lot of initial info using ( -q ) and -p can be used to attach it to processes. -h for printing out its help menu and so on.



Some Useful Functions in GDB are :

1. disas / disassemble [function name here] : To disassemble a function of a certain name.
2. break [ by function name ] / break *0xAddress - To put breakpoints in our code at the entry of function or a certain address.
3. print [name/register/variable] : This can be used to print out the value of a function, register or var.
4. info [name] : This will display information about a certain thing you tell it, Like info registers will print all registers.
5. step : Step to the next source line in the code.
6. stepi : Step into exactly one instruction
7. x - examine : This can be used to display various memory locations in various formats.

```bash
x/[number of units][data type][location name]
```
8. To display n number of words after a instruction we can use

```bash
# In case n is 10
x/10w $eip
```

9. To display n number of instructions starting from a certain register, instruction.
```bash
# in case of n= 20
x/20i $eip
```
To do a basic Binary Exploitation check with gdbpeda we can use the following syntax :
```bash
gdb <binaryname>
r # To run the binary
# Lets get some payloads  with gdb-peda  we can use pattern create
pattern_create 150 # We can now run he program with this output we got and the RSP,ESP first 4 letters are what we will we have to analyze
pattern_offset <thefourbytes> # and we will get the offset
```

Other Useful tools : readelf, ltrace, strace, strings and objdump.

1. ltrace and strace are used to trace library and system calls respectfully performed by the binary.
2. readelf displays information about the executable file itself.
3. objdump displays information about object files , it can also disassemble Linux executables.
4. strings this is to just extract all the readable strings from a binary , this is useful if something is lets say hardcoded in a binary.


Also a great way we should try to analyze stuff is by using Ghidra to analyze the binary, you can also use r2 but Ghidra analyses the code and puts it in C which makes it easier to understand it.
A great resource to learn this is : https://tryhackme.com/room/ccghidra and we can also use Pwntools library to basically help us with the exploitation of this process.
