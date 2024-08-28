[return to table](../README.md)

---


## Add A User

```
# Add User (-m will create the home dir)
sudo useradd -m <username>

# Set Password
sudo passwd <username>

# (Optional) Create a Home Directory (if not automatically created)
sudo mkdir /home/<username>
sudo chown <username>:<username> /home/<username>
```


## Remove A User

```
## Delete user only
sudo userdel <username>

## Delete user and their home dir
sudo userdel -r <username>
```

