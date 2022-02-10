# H4cked THM
Find out what happened by analysing a .pcap file and hack your way back into the machine

### wireshark analysis
- user logged into ftp using user: jenny and password: password123
- uploaded shell.php to /var/www/html
- gained access via reverse shell and downloaded backdoor rootkit Reptile 

### Getting back into the machine
- Using hydra to crack password changed by attacker
	- user: jenny
	- password: 987654321

- Log in using credentials via ftp

- Update shell.php to get reverse shell on my machine
	- get shell.php (ftp command to download file)
	- change ip address on downloaded shell.php (local)
	- put shell.php (ftp command to upload file)

- get reverse shell
	- open netcat listener 
		- nc -lvnp 80
	- visit http://<ip>/shell.php

- stabilize shell
	- python3 -c 'import pty; pty.spawn("/bin/bash")'

- escalate priveleges
	- sudo su jenny using same credentials as above, then
	- sudo su gets root

- flag is in /root/Reptile
