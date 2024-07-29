# twhelan25-tryhackme--Brooklyn-Nine-Nine
This is a detailed walkthrough for the tryhackme CTF Brooklyn Nine Nine. As stated in the tryhackme introduction there are two paths you could take and I will take one. I will not provide any flags or passwords as this is intended to be used as a guide.

## Scanning

First off, let's store the target IP as a variable for easy access.

Command: export ip=xx.xx.xx.xx

Next, let's run an nmap scan on the target IP:
```bash
nmap -A -v $ip -D RND:10 -oN nmap.txt
```

Command break down:

-A: This flag enables aggressive scanning. It combines various scan types (like OS detection, version detection, script scanning, and traceroute) into a single scan.

-v: increases verbosity, providing more detailed output during the scan.

—$ip: provides the target IP we stored as the variable $ip.

-D RND:10 Nmap can send additional packets to confuse network intrusion detection systems (IDS) or hide the true source of the scan by randomly selecting up to 10 decoy IP addresses.

-oN nmap.txt: This option specifies normal output that should be saved to a file named “nmap.txt.

This scan reveals two open ports:

![nmap](https://github.com/user-attachments/assets/78e38f1f-999e-4f08-8523-911acde1ae92)

Notice ftp allows anonymous login.

# Enumeration

### **Analysis Port 80/TCP**

Let's check out the websever on port 80/TCP.

I checked the source code and revealed this html comment:

![steg](https://github.com/user-attachments/assets/a44d6f94-5da2-44d0-aeea-2a41c7d2c59a)

Let's take this hint and open the image in a new tab, and download the image. I can exiftool on the image to see if there is any meta data, but there isn't. Next, let's try running the tool steghide hide on it:

![steghide](https://github.com/user-attachments/assets/f888aae6-efdc-4a20-915b-13ce65827697)

We are prompted for a password that we don't yet have. So, let's run stegcracker on the image:

```bash
stegcracker brooklyn99.jpg
```
After a few moments I got the password! Now, let's run the previous steghide command and use this password to see what the image is hiding. 

We now have CPT Holt's password! As the nmap scan revealed, port 22 ssh is open, so let's try to log into ssh as holt:

```bash
ssh holt@$ip
```
# Gaining Privileges

We're in! In holt's home directory, we now have user.txt.

Now let's run sudo -l to see holt's permissions:

![sudo -l](https://github.com/user-attachments/assets/dfdb4654-5d19-4999-b96a-2db0400f8384)

Now, let's head over to gtfobins and search for sudo nano:

![sudo nano](https://github.com/user-attachments/assets/a526c8c4-ad79-4c7e-bbcc-836bd648bf82)

# Exploitation

The exploit worked and we get root! From here we can just run the command to reveal root.txt

```bash
cat /root/root.txt
```
I hope you enjoyed this hacking the nine nine. Be sure to try to figure out the other path that you can take to root this box.




