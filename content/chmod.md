[return to table](../README.md)

---

## Change Permission: ```chmod```

For applying ```chmod``` on all subpaths (-R). For example:
```
chmod -R 777 [path]
```

Frequently used codes:
- ```700```: Only the owner can read/write/execute.
- ```777```: Everyone can read/write/execute.

## Change Ownership: ```chown```

```
chown -R [owner]:[group] [path]
```

For example:
```
chown -R peizhi:peizhi /mnt/peizhi-ssd
```

