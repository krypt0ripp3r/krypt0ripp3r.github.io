# Hack the box - Netmon

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.152</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2019 Mar 2</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Made by: </span><b>mrb3n</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.152
```
![Nmap results](./nmap.png)

We can quickly identify 2 potential targets - FTP login and web server.

## Getting user flag through anonymous FTP

From nmap scan we noticed that FTP server allow anonymous login. Inside we can find sensitive data:

![User flag through FTP](./user_flag_through_ftp.png)

## Privilege escalation through web service

On port 80 we have PRTG Network Monitor login screen:

![NETMON login](./login_page.png)

Since we have ftp access, we could get login credentials using filesystem. Some users tend to leave backup files with sensitive data:

![FTP download backup](./ftp_download_backup.png)

Login credentials found inside:

![Login credentials](./login_credentials.png)

If we try to login with same credentials it won't work, so we can enumerate password numbers little bit with replacing 2018 with 2019. Finally, successful login with prtgadmin:PrTg@dmin2019:

![Login](./login.png)

Let's try to check if this version is vulnerable with searchsploit:

![Searchsploit information](./searchsploit.png)

There is one version that could be potentialy vulnerable so we use that file:

```
searchsploit -x exploits/windows/webapps/46527.sh > exploit.sh
```

Remember to delete first lines of exploit because it won't work. We also have to get cookie in order to pass to the script. We could use burp suite or browser plugins for it. I am using cookie editor plugin:

![Cookie monster](cookie.png)

We could easily run script after:

![Exploitation](exploitation.png)

Exploit created admin user for use with pentest:P3nT3st! credentials. To get the shell, we could use psexec:

![Getting the flag](capturing_root_flag.png)