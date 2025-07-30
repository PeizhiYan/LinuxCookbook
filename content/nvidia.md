[return to table](../README.md)

---

## Install Nvidia Driver on Ubuntu

### Step 1: Clean up broken/partial NVIDIA packages
```
sudo apt purge '^nvidia-.*'
sudo apt autoremove --purge
sudo apt remove --purge linux-modules-nvidia-* nvidia-kernel-common-* 
```

### Step 2: Clean and update APT cache
```
sudo apt clean
sudo apt update
```

### Step 3: Fix missing kernel dependencies
```
sudo apt install linux-headers-$(uname -r) linux-image-$(uname -r)
```

### Step 4: Try installing a known good NVIDIA version

> [!TIP]
> Check the available versions:
> ```
> apt search nvidia-driver
> ```

```
sudo apt install dkms
sudo apt install nvidia-driver-570
```

Reboot and test
```
sudo reboot
```

```
nvidia-smi
dkms status
```





