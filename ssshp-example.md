# Example run of script

```sh
❯ sudo ssshp
1) Make SSH Tunnel
2) Select SSH Tunnel
3) Quit
Select operation: 2
>>> Select SSH Tunnel from saved hosts in /Users/user/.ssh/sshpHosts.json
Select the Host:
1) vps-host1		     3) local-vm
2) webhost			     4) aws-host
Select operation: 4

You selected: aws-host

... Checking for existing SSH Tunnels ...

>>> Attempting connection

Using command: ssh -f -N -M -S /tmp/sshtunnel -D 1080 ubnt@192.0.2.1 -i ~/.ssh/keys/aws-host.pem

>>> Checking for Established SSH Tunnels with: lsof -i tcp | grep ^ssh
ssh       17855                    root    3u  IPv4 0x7fea8d61e973bd3c      0t0  TCP 1.2.0.192:62127->ec2-192-0-2-1.ap-southeast-2.compute.amazonaws.com:ssh (ESTABLISHED)
ssh       17855                    root    5u  IPv6 0x69095ed77ef34194      0t0  TCP localhost:socks (LISTEN)
ssh       17855                    root    6u  IPv4 0x5afee559b726830b      0t0  TCP localhost:socks (LISTEN)

SOCKS settings for active NIC: Wi-Fi
Enabled: Yes
Server: 127.0.0.1
Port: 1080
Authenticated Proxy Enabled: 0

Detected WAN IP addresses

SOCKS Connection for Browser:
192.0.2.1
*TIP: if using curl, type: curl -sS -x socks5h://localhost:1080 http://whatismyip.akamai.com/ ; echo

Your Computers Primary WAN IP:
192.0.2.1

>>> Keep terminal open until you are finished with SOCKS Proxy

When ready select from options below
1) Close Tunnel
2) Save Settings
3) Exit Script
Select operation: 1

>>> Closing SSH Tunnel, Tunring off SOCKS Proxy

Exit request sent.
Enabled: No
Server: 127.0.0.1
Port: 1080
Authenticated Proxy Enabled: 0

Done.
```
