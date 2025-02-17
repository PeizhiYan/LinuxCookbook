[return to table](../README.md)

---

# Pi VPN

## Run the official installation script:
```
curl -L https://install.pivpn.io | bash
```

- Recommand: Choose OpenVPN
- Recommand: Choose UDP protocal
- Recommand: Choose Cloudflare DNS
- Public DNS Name (for example the url from no-ip):  your_custom_name.ddns.net

## Troubleshoot

### Check OpenVPN listening ports
```
sudo ss -tulnp | grep openvpn
```

Expected output:
```
udp        0      0 0.0.0.0:8888          0.0.0.0:*               724/openvpn
```
Where ```0.0.0.0:8888``` means it is listening on the local port ```8888```, ```0.0.0.0:* ``` means it accepts all connected from the Internet, and ```724``` is the PID.


### I can connect to VPN if my client device is connected to the same local network (same router) with the VPN server, however, if my client device is not connected to the same local network, it does not work.

#### Step 1: Check the Pivpn configuration file:
```
sudo nano /etc/openvpn/server.conf
```

Make sure these lines exist (means using Cloudflare DNS):
```
push "dhcp-option DNS 1.1.1.1"
push "dhcp-option DNS 1.0.0.1"
```

If missing, add them and restart the service:
```
sudo systemctl restart openvpn
```

#### Step 2: Check OpenVPN server's DNS
```
sudo resolvectl status
```
If it shows 192.168.0.1 as the DNS, force OpenVPN's DNS by editing:
```
sudo nano /etc/resolv.conf
```

Replace with:
```
nameserver 1.1.1.1
nameserver 1.0.0.1
```
Then restart the service
```
sudo systemctl restart openvpn
```


#### Step 3: Check the generated ```.ovpn``` file:
Make sure these lines exist:
```
dhcp-option DNS 1.1.1.1
dhcp-option DNS 1.0.0.1
```

If missing, manually add them, and re-import to the OpenVPN client.



