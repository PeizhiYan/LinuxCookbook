[return to table](../README.md)

---


## CPU and RAM Quota Setting

```shell
## Set CPU Quota for user-1001 to 50%
sudo systemctl set-property user-1001.slice CPUQuota=50%

## Reset CPU Quota for user-1001 to unlimited
sudo systemctl set-property user-1001.slice CPUQuota=

## Set RAM Quota for user-1001 to 4GB
sudo systemctl set-property user-1001.slice MemoryMax=4G

## Check
sudo systemctl show user-1001.slice --property=CPUQuota,MemoryMax
```

## GPU Quota Setting

```shell
## Check the Maxumum Power
sudo nvidia-smi -q -d POWER

## Set
sudo nvidia-smi -i <GPU> -pl <POWER>
```

