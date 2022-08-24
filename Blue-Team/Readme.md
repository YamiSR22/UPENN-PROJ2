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



