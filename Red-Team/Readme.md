# Red-Team Security Assessment 
## Recon: Describing the Target

|              VMs              |  IPv4 Address |       Role on Network      |
|-------------------------------|---------------|----------------------------|
| Azure Hyper-V ML-RefVm-684427 | 192.168.1.105 | NatSwitch                  |          
| Kali                          | 192.168.1.90  | Penetration Testing System |          
| Capstone                      | 192.168.1.105 | Web Server                 |          
| Elk-Stack                     | 192.168.1.100 | SIEM System                | 

##Vulnerability Assessment

###The assessment uncovered the following critical vulnerabulities in the target:

| Vulnerability                 | Description   |       Impact      |
|-------------------------------|---------------|-------------------|
| Persistent Reverse Shell Backdoor | Able to deploy reverse shell payload exploit on web server as IPS/IDS/Firewall(s) allowing the outbound ports and undetected reverse shell. |     Can gain remote backdoor shell access to Capstone Apache web server.   |          
| Kali                          | 192.168.1.90  |    Linux 2.6.32   |          
| Capstone                      | 192.168.1.105 |       Linux       |          
| Elk-Stack                     | 192.168.1.100 |       Linux       | 
