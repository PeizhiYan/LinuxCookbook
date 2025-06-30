[return to table](../README.md)

---




## Check Processes of a User
```shell
ps -fu <USER>
```

## Check the Details of a Process
```shell
ps -fp <PID>
```

If it shows
```csharp
UID          PID    PPID  C STIME TTY          TIME CMD
<USER>   2693019 2693018 99 Jun18 pts/30   1-02:37:39 [python] <defunct>
```
> [!NOTE]
> ```<defunct>``` means the process ```2693019``` is dead (zombie🧟), it has a parent process ```2693018```
> Only killing the parent process ```kill -9 <PID>``` can kill the zombie process.
>

## Halt, Resume a Process

### Halt

```Ctrl``` + ```Z```

### Check Jobs

```bash
jobs
```

Output looks like:
```csharp
[1]+  Running                 some_command &
[2]-  Stopped                 another_command
```

### Resume

Resume the first job in foreground
```bash
fg
```

Resume a job by job ID (e.g., [2])
```bash
fg %2
```

Resume a job in background
```bash
bg
```







