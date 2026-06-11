# CTF_IDOR-writeups
This is where I will be inputting my CTF reports
 ## Corridor
This TryHackMe challenge is another easy one where we find our way back to where we came from. This looks fun and I'm excited to start. We will be exploring potential IDOR Vulnerabilities, which are a type of access control vulnerability that arises when an application uses user-supplied input.
Step 1: Basic Nmap scan discovered the website, then an HTTP lookup to get the link using:
`http://10.64.183.128`
- On the website, each room that was shown had a different ending sequence for the URL.
- I inspected the page on the website to get the full list of each room's hexadecimal sequence. Since all are 32 characters, which we know is an MD (Message Digest), this cryptographic hash is common, so to start, I went with the newer version, which is MD5.
Using CyberChef, I went through the first 13 sequences since there are 13 rooms. Each of those 13 outputs matches each URL hexadecimal sequence tied to each room.
In this lab, we can make an educated guess that the undiscovered location is likely a hidden 14th room, so with that, we can first put `14` in the input and see what that gives you. That showed an unknown website, so instead I tried `0` which was the correct input, and with that output I put it in the website's URL and found the flag.
 ## Cipher Secret Message
 Step 1 — Import Encryption algorithm in Python

Check Python version
The website challenge provided us with how the message was encrypted. Using that, we can paste it into Notepad and rewrite it so we can decrypt the message like the website gives us the lines to derive/join the encoded characters together, and the encryption logic which is add 1.

Since we have that, we just do the opposite and subtract 1 and change the plaintext to ciphertext.

Since that's completed, all that's left is to save it, then use Python to execute the decryption code and there you have it, you found the flag.
 ## Brooklyn99 
 Step 1 — The details related to this project are very limited, so I will have to begin using my own knowledge. Using the Nmap (T4 scan) I was able to find these open ports.
Open ports found:
•	21/tcp — FTP
•	22/tcp — SSH
•	80/tcp — HTTP
Step 2 — Let's look at port 80 since it's clearly not secured, using: curl -I http://(IP):80 This just shows the body tied to the targeted HTTP. -I specifies things like cookies, headers, and other data from the results of said scan.
•	Discovered the server is running Apache 2.4.29
•	Found an image related to the challenge: brooklyn99.jpg — that's very promising, so let's head over to the website and look up the version to see if any vulnerabilities are related.
•	Nothing found for the Apache version as of now.
•	After reviewing the inspected HTML, I found a message that says "Have you ever heard of steganography?" — this alone is enough for me to download the image and use Steghide.
Running steghide extract -sf on the image extracts any hidden data. -sf specifies the file.
•	Got asked for a passphrase — going to skip for now.
•	Using the FTP port to try and find the flag. Connected with ftp (IP), and the username was anonymous, the password was just Enter. Found a file: note_to_jake.txt
Try 3 — SSH port 22. We have a name and a known weak password. Let's try: ssh jake@(IP)
Try 4 — This got us into the terminal where the password type section is present. Let's figure the password out. We also got a key fingerprint SHA character string.
•	cat note_to_jake.txt to get the content of the file.
Try 5 — Used Hydra to brute force and look for Jake's password, specifically using rockyou, and found the password: 987664321
Try 6 — In Jake's account, we just run basic reconnaissance, looking through his home directory and files. I found two names while searching, Holt and Amy.
Try 7 — Since nothing was found in Amy's home directory, I went to Holt and found nano.save and user.txt. After reviewing that file, I found a 32-character string that was the 1st flag.
Try 8 — Now the root password. In the previous directory, there was .sudo_as_admin_successful. We will use that file to find the root flag using a sudo command.
Try 9 — I am now in the root terminal using: sudo /usr/bin/less /etc/passwd then !/bin/bash This is where we use the password obtained in Try 5 to access the root terminal.
Try 10 (final) — After I obtained root.txt, I ran cat /root/root.txt and retrieved the final flag.

