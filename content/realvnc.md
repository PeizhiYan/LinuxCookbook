[return to table](../README.md)

---

## Install RealVNC Server

https://www.realvnc.com/en/connect/download/vnc/


## Start RealVNC Server

### Restart service
```
sudo systemctl restart vncserver-x11-serviced
```

### Enable on boot
```
sudo systemctl enable vncserver-x11-serviced
```

### Check status
```
systemctl status vncserver-x11-serviced
```

Should see something like this:
```
● vncserver-x11-serviced.service - VNC Server in Service Mode daemon
     Loaded: loaded (/lib/systemd/system/vncserver-x11-serviced.service; enabled; preset: enabled)
     Active: active (running) since Sun 2025-02-16 22:42:46 PST; 1min 52s ago
   Main PID: 649 (vncserver-x11-s)
      Tasks: 2 (limit: 760)
        CPU: 171ms
     CGroup: /system.slice/vncserver-x11-serviced.service
             ├─649 /usr/bin/vncserver-x11-serviced -fg
             └─675 /usr/bin/vncserver-x11-core -service

Feb 16 22:42:46 pi systemd[1]: Started vncserver-x11-serviced.service - VNC Server in Service Mode daemon.
Feb 16 22:42:47 pi vncserver-x11[675]: ServerManager: Server started
Feb 16 22:42:47 pi vncserver-x11[675]: ConsoleDisplay: Cannot find a running X server on vt1
Feb 16 22:42:50 pi vncserver-x11[675]: ConsoleDisplay: Cannot find a running X server on vt7
```

If not, try reboot the system, it will sometimes solve the problem.

## To login RealVNC account (X11 UI)
```
vnclicensewiz
```


