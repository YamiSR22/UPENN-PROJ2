# Blue-Team Log Analysis and Attack Characterization #

## Analysis: Identifying the Port Scan ##

* <b>What time did the port scan occur?<b>
    * 07/25/2022 @12:00
* <b>ow many packets were sent, and from which IP?<b>
    * 32,452 from the source IP 192.168.1.90
* <b>What indicates that this was a port scan?<b>
    * The sudden peaks in network traffic indicate that this was a port scan.
    
<img width="388" alt="Connections-over-time-ECS" src="https://user-images.githubusercontent.com/100730516/186331861-9e7c826d-cb7b-4c25-afe5-147a724ab26c.png">

## Analysis: Finding the Request for the Hidden Directory ##

* <b>What time did the request occur? <b>
    * The request started at 11:00 on 07/25/2022
* <b>How many requests were made?<b>
    * About 16,262 requests were made to access the /secret_folder
* <b>Which files were requested? <b>
    * The /secret_folder contained hash that could be used to access the system using another employee’s credentials which was Ryan.
* <b>What did they contain?<b>
    * The /secret_folder allowed me to upload a payload – exploiting other vulnerabilities.
    
<img width="334" alt="TOP-10-HTTP-REQUESTS" src="https://user-images.githubusercontent.com/100730516/186332194-0cf1d2ed-6826-4297-9608-6ea80def4c6f.png">

## Analysis: Uncovering the Brute Force Attack ##

* <b>How many requests were made in the attack?<b>
    * 16,262
* <b>How many requests had been made before the attacker<b>
    * 16,262
    
<img width="347" alt="TOP-10-HTTP-REQUEST-SECRET-FOLDER" src="https://user-images.githubusercontent.com/100730516/186332608-2eeb6437-ef36-4fb3-a3d3-0b513edbf5b7.png">
  
## Analysis: Finding the WebDAV Connection ## 

* <b>How many requests were made to this directory?<b>
    * 29
* <b>Which files were requested? <b>
    * Passwd.dav and shell.php
<img width="332" alt="HTTP-TRANSACTIONS" src="https://user-images.githubusercontent.com/100730516/186333064-90f8e85c-0f69-47df-aa0b-25a40b64b5a4.png">

<img width="371" alt="Screenshot 2022-08-23 202949" src="https://user-images.githubusercontent.com/100730516/186333109-3f63ddd4-d186-4014-9078-31cf206584fa.png">


<img width="449" alt="shell-php-in-lhost" src="https://user-images.githubusercontent.com/100730516/186333135-b1ee8905-2376-4efc-9ce8-2802c9ea1502.png">

# Blue Team Proposed Alarms and Mitigation Strategies #

## Mitigation: Blocking the Port Scan ##
   
<b>Alarm<b>
* What kind of alarm can be set to detect future port scans?
   * Setting an alert be sent once 1000 connections occur in an hour.
* What threshold would you set to activate this alarm?
   * Alert email and log when > 3 none port 403 or port 80 scans detected at the same timestamp from the IP occur

<b>System Hardening<b>
* What configurations can be set on the host to mitigate port scans?
   * Set server IPtables to drop packet traffic when thresholds are exceeded
   * Ensure the firewall is regularly patched to minimize new zero-day attack
   * Regularly run a system port scan to proactivity detect and audit any open ports
   * Ensure the firewall detects and cuts off the scan attempt in real time.

## Mitigation: Finding the Request for the Hidden Directory ##

<b>Alarm<b>
* What kind of alarm can be set to detect future unauthorized access?
   * Setting an alert when future unauthorized access occur.
* What threshold would you set to activate this alarm?
   * Maximum if 5 attempts per hour that would trigger an alert to be sent.
   
<b>System Hardening<b> 
 * What configuration can be set on the host to block unwanted access? 
   * Confidential folders that shouldn’t be shared for public access
   * Encrypting data that is contained within confidential folders
   * Renaming folders that contains sensitive/private/company critical data
   * Reviewing IP addresses that causes an alert to be sent: either whitelist or block the IP addresses. 
   
## Mitigation: Preventing Brute Force Attacks ## 

<b>Alarm<b>
* What kind of alarm can be set to detect future brute force attacks?
   * Setting an alarm that alerts if a 401 error is returned when detecting future brute force attacks.
   
<img width="371" alt="Screenshot 2022-08-23 203149" src="https://user-images.githubusercontent.com/100730516/186334768-36fea6b4-aebe-4cb3-9614-4f2ef0f94e24.png">   
   
* What threshold would you set to activate this alarm?
   * Alert email and log when, on protected files and folders, > 5 Error (401) responses occur at any time OR any OK (200) responses occur from non-trusted IPs.
   
<b>System Hardening<b>
* What configuration can be set on the host to block brute force attacks?
   * Create a password policy that requires password complexity. Then compare the passwords to common passwords lists and prevent users from reusing historical passwords.
   * Create a policy that locks out accounts for 30 mins after 5 unsuccessful attempts.
   * Creating a blocked IP based on IP addresses that have 25 unsuccessful attempts in 4 months.
   
## Mitigation: Detecting the WebDAV Connection ##

<b>Alarm<b>
* What kind of alarm can be set to detect future access to this directory?
   * Create a whitelist of trusted IP Addresses then review the list every 5 months if these users really need access
   * Setting an alarm on HTTP request, in which it activates on any IP address that is trying to access the webDAV directory outside of those trusted IP addresses. 
* What threshold would you set to activate this alarm?
   * Alert email and log when requests are made, on protected files and folders, from non-trusted IPs.
   
<b>System Hardening<b>
* What configuration can be set on the host to control access?
   * Any access to the webDAV folder is only permitted by users with complex username and passwords when it comes to conjunction with other mitigation strategies.
   * Ensuring that the firewall security policy prevents all other access and creating a whitelist of trusted IP addresses.

## Mitigation: Identifying Reverse Shell Uploads ## 
   
<b>Alarm<b>
* What kind of alarm can be set to detect future file uploads?
   * Setting an alert that can detect any traffic attempting to access port 4444.
   * Setting an alert for any files being uploaded into the /web/webDAV folder.
* What threshold would you set to activate this alarm?
   * Alert email and log when “put” request methods are made, on protected folders, from non-trusted IPs

<b>System Hardening<b>
* What configuration can be set on the host to block file uploads?
   * Ensuring only necessary ports are open
   * Blocking all IP addresses other than whitelisted IP addresses
   * Giving permission to only read the /webDav folder, this prevent payloads from being uploaded
