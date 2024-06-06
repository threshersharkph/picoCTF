#Author: Jake Beley
#Description
#Your goal is to find the flag. compress_and_attack.py nc mercury.picoctf.net 29858

```#from apoirrier

from pwn import *
import string

sh = remote("mercury.picoctf.net", 29858)

def oracle(text):
    sh.recvuntil("encrypted:")
    sh.sendline(text)
    sh.recvline()
    sh.recvline()
    return int(sh.recvline().decode())

known = "picoCTF{"  #you need to run this 2 - 3 times adding the found letters or _

length = oracle(known)
print(known, end="")

current = ""
while current != "}":
    for c in string.printable:
        if oracle(known + c) == length:
            known += c
            current = c
            print(c, end="")
            break
```
