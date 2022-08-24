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
