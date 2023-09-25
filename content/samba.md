[return to table](../README.md)

---

## Setup SAMBA File Sharing Service

### Installing SAMBA
```
sudo apt update
sudo apt install samba
```
### Setting Up SAMBA
(The configuration file for Samba is located at ```/etc/samba/smb.conf```)
```
sudo nano /etc/samba/smb.conf
```
At the bottom of the file, add:
```
[cloud_drive]
    comment = SambaShare
    path = /home/username/sambashare
    read only = no
    browsable = yes
```

### Restart to Take Effect
```
sudo service smbd restart
```

### Update Firewall Rule
```
sudo ufw allow samba
```
