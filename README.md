## Capstone Engagement

This document describes the successful penetration test of a Capstone webserver.

While attacking the victim machine, the team set up an ELK stack to log the requests by the server for future analysis to harden the system against similiar attacks in the future.

Goals

-Enumerate IP address and vulnerabilities on the victim machine.
-Exploit vulnerabilities to gain access.
-Find hidden files on the machine.
-Enable future access to the victim machine -Persistance. 


## Network Topology

All machine's involved in the engagement were hosted in an Azure cloud environment Utilizing Hyper-V, ELK, and Kali Linux.
This was a Grey Box penetration test as we had some information about the machine that we were attacking. The vicim machine had Metricbeat and Filebeat installed and was forwarding all logs to the ELK machine.


Machines

Host Machine
Windows OS
IPV4: 192.168.1.1

Attack Machine
Kali Linux
IPV4: 192.168.1.90

Victim Machine
Linux
IPV4: 192.168.1.105

ELK Machine
Linux
IPV4: 192.168.1.100

![alt text]https://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/Capstone%20Network%20Topology.png

## Reconnaissance

Enumeration with NMAP
Using the popular and effective tool NMAP, we were able to enumerate the topology of the environment. We were able to view the IP addresses of the machines on the network, and view the open ports. Our team noticed that the victim machine had both port 22 and 80 sitting open. This means that we would be able to access the machine using either an SSH connection, or by using HTTP.

![alt text]://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/NMAP.png

Knowing that the IP address had port 80 open, we used a Chrome web browser to nagivate to view the web site that the IP was hosting- 

http://192.168.1.105 

## Exploitation

Through manual enumeration, we were able to discover directories that should have been hidden. For example, "company_folders/secret_folder" as well as an informational note saying that the user "Ashton" had access to the folder.

At this point we were able to locate the folder, however it was still protected by a password. Using a brute force attack with ORY Hydra, we were quickly able to obtain the password associated with user Ashton.

https://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/Company%20Secret%20Folder.png

https://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/Hydra.png

In the secret folder we found information about how connect to the WebDAV server along with a hashed password of user "Ryan". We needed access to Ryan's account in order to access the WebDAV server so we used a free hashcracking tool found on the website https://crackstation.net. Within minutes we had access to Ryan's account, giving us access to the WebDAV server.

https://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/WebDAV%20instructions.png

## Persistance

Using MSFVENOM we crafted a .php payload that would iniate a reverse shell back to our host machine, giving us full access to the web server. After accepting the reverse shell with a NetCat Listener, we had a shell on the server. After looking around for a few momments we were able to finish our engagement by finding the flag and exporting it to our host machine. 

https://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/Upload%20EXE.png

https://github.com/calvindavis1/Red-Vs.-Blue/blob/main/Images/Red/find%20and%20download%20flag.png
