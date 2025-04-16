[return to table](../README.md)

---

# Bash Useful Examples


## Change Suffix of Multiple Files
```bash
for fname in *.jpg;
  do mv "$fname" "${fname%.jpg}.png";
done
```

