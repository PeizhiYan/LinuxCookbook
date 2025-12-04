[return to table](../README.md)

---


## Add Docker to Groups

To use Docker without requiring `sudo`, add your user to the **docker** group.

```
# Add your user to the docker group
sudo usermod -aG docker $USER
```

Temperary solution (current session only)
```
newgrp docker
```

Verify that Docker is in your groups:
```
groups
```
You should see ```docker``` listed.


