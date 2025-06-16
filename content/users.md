[return to table](../README.md)

---


## Add A User

```shell
# Add User (-m will create the home dir)
sudo useradd -m <username>

# Set Password
sudo passwd <username>

# (Optional) Create a Home Directory (if not automatically created)
sudo mkdir /home/<username>
sudo chown <username>:<username> /home/<username>
```


## Remove A User

```shell
## Delete user only
sudo userdel <username>

## Delete user and their home dir
sudo userdel -r <username>
```


## List Users

```shell
## List all human users
awk -F: '$3 >= 1000 && $3 < 65534 { print $1 }' /etc/passwd
```

> - $3 is the UID (User ID) field in /etc/passwd.
> - $1 is the username.
> - System users (usually UID < 1000).
> - The nobody user (UID 65534).


## Get Detailed Info on a Specific User

```shell
id username
```








