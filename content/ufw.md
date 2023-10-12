[return to table](../README.md)

---

# Uncomplicated Firewall (UFW)


## Open a Port
```
sudo ufw allow [servicename]
sudo ufw enable
```
Here, ```servicename``` can be a port. For example,
```
sudo ufw allow 8080
sudo ufw enable
```

One can also specify which protocal to open for that port:
```
sudo ufw allow [servicename]/tcp
sudo ufw enable
```

---

## Allow Connections from a Specific IP:
```
sudo ufw allow from [IP]
sudo ufw enable
```

---

## Close a Port
```
sudo ufw deny 995
sudo ufw enable
```

---

## Check Open Ports in UFW
```
sudo ufw status 
```





