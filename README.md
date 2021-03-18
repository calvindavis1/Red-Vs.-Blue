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

![Capstone Network Topology](https://user-images.githubusercontent.com/75997449/111559414-1689d100-8756-11eb-9719-4c5ce7314446.png)

## Reconnaissance

Enumeration with NMAP
Using the popular and effective tool NMAP, we were able to enumerate the topology of the environment. We were able to view the IP addresses of the machines on the network, and view the open ports. Our team noticed that the victim machine had both port 22 and 80 sitting open. This means that we would be able to access the machine using either an SSH connection, or by using HTTP.

![NMAP](https://user-images.githubusercontent.com/75997449/111559463-302b1880-8756-11eb-8bbb-ff7498b968a0.png)

Knowing that the IP address had port 80 open, we used a Chrome web browser to nagivate to view the web site that the IP was hosting- 

http://192.168.1.105 

## Exploitation

Through manual enumeration, we were able to discover directories that should have been hidden. For example, "company_folders/secret_folder" as well as an informational note saying that the user "Ashton" had access to the folder.

![Company Secret Folder](https://user-images.githubusercontent.com/75997449/111559490-3de09e00-8756-11eb-9021-1fd14caa394d.png)


At this point we were able to locate the folder, however it was still protected by a password. Using a brute force attack with ORY Hydra, we were quickly able to obtain the password associated with user Ashton.


![Hydra](https://user-images.githubusercontent.com/75997449/111559520-52bd3180-8756-11eb-9363-a63d1dd24b1d.png)

In the secret folder we found information about how connect to the WebDAV server along with a hashed password of user "Ryan". We needed access to Ryan's account in order to access the WebDAV server so we used a free hashcracking tool found on the website https://crackstation.net. Within minutes we had access to Ryan's account, giving us access to the WebDAV server.

![WebDAV instructions](https://user-images.githubusercontent.com/75997449/111559544-623c7a80-8756-11eb-893f-331e55d6c1c6.png)

## Persistance

Using MSFVENOM we crafted a .php payload that would iniate a reverse shell back to our host machine, giving us full access to the web server. After accepting the reverse shell with a NetCat Listener, we had a shell on the server. After looking around for a few momments we were able to finish our engagement by finding the flag and exporting it to our host machine. 

![Upload EXE](https://user-images.githubusercontent.com/75997449/111559599-73858700-8756-11eb-970b-662310451f17.png)

![find and download flag](https://user-images.githubusercontent.com/75997449/111559627-7f714900-8756-11eb-9fb3-8256f19347de.png)
