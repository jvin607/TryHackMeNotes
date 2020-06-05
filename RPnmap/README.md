#RP: Nmap

```
Joe Vinyard | 20200603
```
#IP Address
```
export IP=10.10.38.153
```
#Task 1: Deploy Machine
```
no answer needed
```
#Task 2 Nmap Quiz
1. First, how do you access the help menu?
```
no answer needed

nmap -h
```
2. Often rferred ot as a stealth scan, what is the first switch listed for a 'Syn scan'?
```
-sS
```
3. Not quite as useful but how about a 'UDP Scan'?
```
-sU
```
4. What about operating system detection?
```
-O
```
5. How about service version detection?
```
-sV
```
6. Most people like to see some output to know that their scan is actually doing things, what is the verbosity flag?
```
-v
```
7. What about 'very verbose'? (A personal favorite)
```
-vv
```
8. Sometimes saving output in a common document format can be really handy for reporting, how do we save output in xml format?
```
-oX
```
9. Aggressive scans can be nice when other scans just aren't getting the output that you want and yo ureally don't care how 'loud' you are, what is the switch for enabling this?
```
-A
```
10. How do I set the timing to the max level, something called 'Insane'?
```
-T5
```
11. What if I want ot scan a specific port?
```
-p
```
12. how about if I want to scan every port?
```
-p-
```
13. What if I want to enable using a script form the nmap scripting engine?  For this, just include the first partof the switch without he specification of what script to run.
```
--script
```
14. What if I want to run all scripts out of the vulnerability category?
```
--script vuln
```
15. What switch should I include if I don't want to ping the host?
```
-Pn
```

#Task 3 Nmap Scanning
1. let's go a head and start with the basics and perform a syn scan on the box provided.  What will this command be without the host IP address?
```
nmap -sS

sudo nmap -sS -oN nmap/synScan $IP
[sudo] password for joe: 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-03 14:57 EDT
Nmap scan report for 10.10.38.153
Host is up (0.21s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

```
2. After scanning this, how many ports do we find under 1000?
```
2
```
3. What communication protocol is given for these ports following the port number?
```
tcp 
```
4. Perofrm a service version detection scan, what is the version of the software running on port 22?
```
6.6.1p1

nmap -sV -oN nmap/serviceVersion $IP
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-03 14:59 EDT
Nmap scan report for 10.10.38.153
Host is up (0.23s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.34 seconds

```
5. Perform an aggressive scan, what flag isn't set under the results for port 80?
```
httponly

nmap -A -oN nmap/aggressive $IP
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-03 15:00 EDT
Nmap scan report for 10.10.38.153
Host is up (0.23s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 8b:bf:21:3c:8b:51:2a:46:ea:23:88:0e:86:5e:b2:20 (DSA)
|   2048 d2:79:9a:78:34:dc:2d:ad:76:ec:7a:6a:33:dc:74:65 (RSA)
|   256 9d:9e:9b:a6:ce:8f:d0:81:1c:0b:3c:77:a7:66:0c:3d (ECDSA)
|_  256 18:68:11:25:fe:12:6f:f9:ee:27:0c:b9:cc:09:0e:af (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-robots.txt: 1 disallowed entry 
|_/
| http-title: Login :: Damn Vulnerable Web Application (DVWA) v1.10 *Develop...
|_Requested resource was login.php
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.68 seconds

```
6. Perform a script scan of vulnerabilities associated with this box, what denial of service (DOS) attack is this box susceptible to? Answer with the name for the vulnerability that is given as the section title in the scan output.  A vuln scan can take a while to complete.  In case you get stuck, the answer to this question is provided in the hint, however, it's goodto still run this scan and get used to using it as it can be invaluable.
```
http-slowloris-check

nmap --script vuln -oN nmap/vuln $IP
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-03 15:02 EDT
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.38.153
Host is up (0.21s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
80/tcp open  http
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|       httponly flag not set
|   /login.php: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /login.php: Possible admin folder
|   /robots.txt: Robots file
|   /config/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|   /docs/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|_  /external/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.

Nmap done: 1 IP address (1 host up) scanned in 377.85 seconds

```
