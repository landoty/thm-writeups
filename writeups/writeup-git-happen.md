# GIT Happens
THM Easy Box : Boss wanted me to create a prototype, so here it is! We even used something called "version control" that made deploying this really easy!

### Enumeration

- Scan with nmap
	- nmap -sC -sV <ip>
	- only port 80 but we see that a .git repository exists

- check webpage
	- simple login portal as default
	- navigating to <ip>/.git/ we can see the git contents

### Exploit

- Download git contents
	- wget --mirror -I .git <ip>/.git/
		- mirror recursively downloads the ENTIRE contents
		- -I .git specifies the files to get

- Explore contents
	- all contents have been deleted
	- git restore command getse most recent commit state back

	- when observing the index.html file, the login script has been obfuscated
	
- check git log 
	- commit 395e087334d613d5e423cdf8f7be27196a360459 "made the login page"
	- commit e56eaa8e29b589976f33d76bc58a0c4dfb9315b1 "obfuscated the source code"
	- commit bc8054d9d95854d278359a432b6d97c27e24061d "they wanted me to use something called "SHA-512"
	
	- need to revert back to pre-obfuscation and hashing
	- git checkout 395e087334d613d5e423cdf8f7be27196a360459 will restore to when the login page was made

- find flag
	- password in plaintext in the login function 
	- Th1s\_1s\_4\_L0ng\_and\_S3cur3\_P4ssw0rd!
