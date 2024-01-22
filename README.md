<h1>Metasploitable 2 Website Hacking</h1>

<h2>Description</h2>

The project consists of using the information from the [enumeration project](https://github.com/ntieu4328/Metasploit-2-Enumeration) to start exploiting the Metasploitable 2 system. In this project I will specifically exploit the website portion of the Metasploitable 2 system.

<h2>Walk-through:</h2>

From the enumeration that was done, I can see that port 80 is open:

![nmap_LI](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/ea2efe3d-e993-4d0d-be56-c34ece7b27e4)

This port is the default port used for HTTP. HTTP is used for unencrypted web pages. HTTPS, which is run on port 443, is used for encrypted web pages.

We can look at the web page by inputting the IP into a web browser:

![metasploitable website](https://github.com/ntieu4328/Metasploitable-2-Website-Hacking/assets/156137990/837fb511-bb84-4078-b85b-da9d884f63a7)
