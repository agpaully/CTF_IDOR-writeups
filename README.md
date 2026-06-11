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
