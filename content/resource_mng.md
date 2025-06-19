[return to table](../README.md)

---


## CPU and RAM Quota Setting

> Note: CPUQuota percentage is basde on a single core.
> for example, if CPUQuota=100%, on a system with 8 logical cores, it means
> allowed CPU is 100% of one logical core, basically 1/8 of the total CPU
> power! 

```shell
## Set CPU Quota for user-1001
sudo systemctl set-property user-1001.slice CPUQuota=200%

## Reset CPU Quota for user-1001 to unlimited
sudo systemctl set-property user-1001.slice CPUQuota=

## Set the Number of Threads User can use (e.g., threads from 0 to 60)
sudo systemctl set-property user-1001.slice AllowedCPUs=0-60

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

### Reset GPU
```
## e.g., free GPU 2
sudo nvidia-smi --gpu-reset -i 2
```

