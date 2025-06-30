[return to table](../README.md)

---

# Useful Tips

## To prevent pip from upgrading or downgrading your other packages when you install a new package
```bash
pip install <package_name> --no-deps
```


## Install a Local Package

```bash
pip install -e .
```
> ```-e``` indicates editable, it lets you work on your package "live" as you develop.

