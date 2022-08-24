# Red-Team Security Assessment #
## Recon: Describing the Target ##

|              VMs              |  IPv4 Address |       Role on Network      |
|-------------------------------|---------------|----------------------------|
| Azure Hyper-V ML-RefVm-684427 | 192.168.1.105 | NatSwitch                  |          
| Kali                          | 192.168.1.90  | Penetration Testing System |          
| Capstone                      | 192.168.1.105 | Web Server                 |          
| Elk-Stack                     | 192.168.1.100 | SIEM System                | 

## Vulnerability Assessment ##

### The assessment uncovered the following critical vulnerabulities in the target: ###

| Vulnerability                     | Description   |       Impact      |
|-----------------------------------|---------------|-------------------|
| Persistent Reverse Shell Backdoor | Able to deploy reverse shell payload exploit on web server as IPS/IDS/Firewall(s) allowing the outbound ports and undetected reverse shell. |  Can gain remote backdoor shell access to Capstone Apache web server.   |          
| Weak Passwords and No Failed Password Lockout | Weak password was found in dictionary “rock you”. There were NO lockout for failed attempts allowing brute force attack. |   The brute force was provide to: /secret_folder/ Password hash for Ryan dav://192.168.1.105/webdav/ |          
| CVE-2019-6579: 80/TCP             | An attacker with network access to the web server on port 80/TCP or 443/TCP could execute system commands with administrative privileges and with network access to the affected service. | Successful exploitation of the security vulnerability compromises confidentiality, integrity or availability of the targeted system.|          
| Directory Listing Enabled on Apache Web Server | Is able to use the browser to read full contents of directories on Capstone Apache web server. | Files were revealed, which shows Ashton as the administrator for the directory: /company_folders/secret_folder/ | 

## Exploitation: Nmap - Open Port 80 ##

* <b>Tools & Processes:<b>

  * I exploited the vulnerability by using Nmap. An Nmap scan shows which ports are open and its service. In this case, when doing an Nmap scan, it showed that port 80/tcp and 22/tcp were open. When typing in the ip address of the machine on a web browser, I was able to gain access to the server with the company’s directories.
  
* <b>Achievements:<b>

  * I was able to identify the IP address and the exposed services of the target VM.
I was also able to gain access to sensitive information and was able to discover a path to a secret folder.

<img width="490" alt="SEARCH-IP-BROWSER" src="https://user-images.githubusercontent.com/100730516/186330033-7b217fdb-857d-470d-b4bf-c79fe8afa2b6.png">

<img width="304" alt="kali-nmap-ip" src="https://user-images.githubusercontent.com/100730516/186330086-66ac2f1e-41ad-4cfa-983d-3caaa1600021.png">

<img width="430" alt="SECRET-FOLDER-FILE" src="https://user-images.githubusercontent.com/100730516/186330183-26e168de-8d90-41a4-b00b-c0904cad8a67.png">
 
## Explotation: Authentication Management ##

* <b>Tools & Processes:<b>

  * I exploited the vulnerability by using Brute Force Attack on the password for the hidden directory by using Hydra. I used Ashton’s name, ran the Hydra attack against the directory. The tool I used to crack the hashed password I used both; the CrackStation website and John-the-ripper. 

* <b>Achievements:<b>  

  * The exploitation provided me with the username of ‘ashton’ as well with the password ‘leopoldo’  
 
<img width="677" alt="CRACKING-HASH" src="https://user-images.githubusercontent.com/100730516/186330498-5f8fa549-b40c-4a4c-9f6a-d8a47e1584a8.png">

<img width="440" alt="HYDRA-PASSWORD-USRNAME" src="https://user-images.githubusercontent.com/100730516/186330565-08df324a-96bf-4063-b224-539ff3a0b5af.png">
 
<img width="321" alt="john-password" src="https://user-images.githubusercontent.com/100730516/186330634-c078a51f-c341-4cc0-a4f0-9bed9bb38f71.png">

## Exploitation: LFI Vulnerability ##

* <b>Tools & Processes:<b>

  * I exploited the vulnerability by using msfvenom and meterpreter to deliver a payload onto the vulnerable machine; the Capstone Server.
  
* <b>Achievements:<b>  

  * By using the multi/handler exploit, I was successfully able to get access to the machine’s shell.  
  
<img width="359" alt="ryan-loggin" src="https://user-images.githubusercontent.com/100730516/186330997-61b500bf-f6aa-4c90-801e-3b4ba89803c0.png">
  
<img width="371" alt="setting-up-exploit" src="https://user-images.githubusercontent.com/100730516/186331017-3c2ee41f-8a61-474e-9685-697a23a19ea3.png">

<img width="419" alt="FLAG-FOUND-USING-METERPRETER" src="https://user-images.githubusercontent.com/100730516/186331071-9cfa8ea3-d9cb-4291-b0e8-2cfc7d7aa55d.png">

 
  
