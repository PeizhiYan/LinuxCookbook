[return to table](../README.md)

---

# Bash Useful Examples


### Change Suffix of Multiple Files
```bash
# in this example, change all .jpg to .png
for fname in *.jpg;
  do mv "$fname" "${fname%.jpg}.png";
done
```

### Run Commands in Parallel
```bash
command1 &
command2 &
command3 &
wait  # waits for all background jobs to finish
```
or
```bash
for i in {1..3}; do
  running_task $i &
done
wait
```
