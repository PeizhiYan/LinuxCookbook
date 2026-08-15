# Tunnel and Proxy

## SSH Tunnel and Proxy

### SSH Local Port Forwarding

```
ssh -L 8888:localhost:6666 [user]@[server]
```
where:
- 8888 is the client's port
- 6666 is the server's port

After running it, `localhost:8888` on client's computer is forwarded to `localhost:6666` on server.

### Dynamic Port Forwarding

It turns your SSH connection into a SOCKS proxy.

The difference with `ssh -L` is that `-L` hardcode one destination so tunnel only ever reaches that one host and port. With `-D 8888`, no destination is baked in. SSH opens a SOCKS server on `localhost:8888`, and whatever connects to it tells SSH where it wants to go. The server will make that connection on your behalf. All the network traffic will go through the server.

```
ssh -D 8888 [user]@[server]
```
Then, setup to use Proxy to browse Internet:
- SOCKS Proxy:
  - Server: 127.0.0.1
  - Port: 8888 

<img width="360" alt="Screenshot 2026-06-11 at 2 29 22 PM" src="https://github.com/user-attachments/assets/708857c7-e3a1-40cc-a5f9-089748f7a52f" />

## Tinyproxy

### Install (on host machine)
```
sudo apt update
sudo apt install tinyproxy
```

### Configuration
```
sudo nano /etc/tinyproxy/tinyproxy.conf
```
Add the following (or uncomment):
```
Port 8888
Listen 0.0.0.0
Allow 127.0.0.1
Allow 192.168.1.123          # this is your client's WAN IP
BasicAuth username password  # this is optional
ConnectPort 443              # this is necessary for HTTPS
ConnectPort 563
```

### Start
```
sudo systemctl start tinyproxy
```
or
```
sudo systemctl start tinyproxy
```
every time the config file changed, need to restart the service

If firewall is used (`sudo ufw status` shows `active`):
```
sudo ufw allow from 192.168.1.123 to any port 8888
```
where `192.168.1.123` is the client's IP.

### Safety

Avoid start after reboot:
```
sudo systemctl disable tinyproxy
```

After use, remember to stop it:
```
sudo systemctl stop tinyproxy
```


