[return to table](../README.md)

---

## Install No-ip Client

https://www.noip.com/download?page=linux


## Start No-ip Client Automatically

### Step 1: Create a new systemd service file
```
sudo nano /etc/systemd/system/noip-duc.service
```

### Step 2: Paste the following information to the file

**Remember to replace the user data**

```
[Unit]
Description=No-IP Dynamic DNS Update Client
After=network.target

[Service]
ExecStart=/usr/local/bin/noip-duc -g [your-no-ip-url] --username [your-no-ip-username] --password [your-no-ip-password]
Restart=always
User=root
StandardOutput=null
StandardError=null

[Install]
WantedBy=multi-user.target
```

### Step 3: Enable and start the service
```
sudo systemctl daemon-reload
sudo systemctl enable noip-duc
sudo systemctl start noip-duc

```

#### check if running
```
systemctl status noip-duc
```

You should see something like this:
```
● noip-duc.service - No-IP Dynamic DNS Update Client
   Loaded: loaded (/etc/systemd/system/noip-duc.service; enabled)
   Active: active (running) since ...
```

