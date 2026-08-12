# SSH

## SSH Config

```shell
sudo nano /etc/ssh/sshd_config
```

### Allow user from accessing
```
AllowUser [username]
```
### Enable access via password
```
PasswordAuthentication yes
```

### Restart SSH to take into effect
```
sudo systemctl restart sshd
```





## Setup Login Linux without Password Everytime

### Step 1
> Use command ```ssh-keygen``` to generate an SSH key on your **local terminal**
> e.g.,
> ```
> linuxsvr01$ ssh-keygen
> Generating public/private rsa key pair.
> Enter file in which to save the key (/home/user/.ssh/id_rsa):
> Enter passphrase (empty for no passphrase):
> Enter same passphrase again:
> Your identification has been saved in /home/user/.ssh/id_rsa.
> Your public key has been saved in /home/user/.ssh/id_rsa.pub.
> The key fingerprint is:
> b2:ad:a0:80:85:ad:6c:16:bd:1c:e7:63:4f:a0:00:15 user@host
> The key's randomart image is:
> +---[RSA 2048]----+
> |           . o + |
> |         = X * . |
> |      . O @ = . .|
> |     + o X O . + |
> |   . = S * = o   |
> | . ..o . . o .   |
> |     + .o. E     |
> |      + ...o.    |
> |     . . ooo+.   |
> +----[SHA256]-----+
> ```

### Step 2
Copy the SSH key to the remote server

> Linux:
> ```
> ssh-copy-id root@linuxsvr02
> ```

> Windows (do it manually):
>
> First, copy the public key (the entire line) in ```C:\Users\[user_name]\.ssh\[the_ssh_key_file].pub```
>
> Then, open the file ```~/.ssh/authorized_keys``` (create the folder and file if not exist) on Linux server, paste the public key to the file, save it. Done.
>
> An example line looks like:
> ```
> ssh-ed25519 AAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBCCCCCCCCCCCCCCCCCCCCC user@address
> ```

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


