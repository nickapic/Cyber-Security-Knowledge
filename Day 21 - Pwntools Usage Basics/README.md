Main Resource Used : https://docs.pwntools.com/en/stable/globals.html 
This is to provide a basic idea and some basic commands to use in Pwntools 

To Download it :
```bash
sudo pip install pwn
```

Most common way to use :
```python
from pwn import *
```

Defining Context of the Binary : 
```python
context(os='linux', arch='amd64')
```
This field is to provide pwntools with context on what architecture, os , bit width etc. are ,this helps when you are lets say trying to generate shellcode, and also in cases you are doing ROP programming, etc. Its always recommened to have this field. A shortcut to set stuff automatically is also : 
```
from pwn import *
context.binary = "./your-vulnbinary)
```
Default values and all the values that you can edit and add can be found here : https://docs.pwntools.com/en/stable/context.html#pwnlib.context.ContextType.defaults

Tubes : 
This is to talk to sockets, ssh connection and processes with pwntools :
```
# Remote sock connection 
remote = ("IP/Host", PORT)
# Attaching Local Processes 
process = process('./binary')
```

ShellCraft :
This is a module you can use to create shellcodes for binaries super easily in case NX is disabled
```python
 shellcode = asm(shellcraft.sh())
```
You can also look at ways to play around with this shellcode generation here : 

Sending Inputs :
Here > is what comes before asking for our input
```
io.sendlineafter('> ', 'whatyouwannasend')
```

ROP Module : 

This tool can be used to build stacks preety trivially. ## More to be added here later ##