[return to table](../README.md)

---


## Clone

### Simple Clone
```bash
git clone <repo_url>
```

### Clone with Submodules
```bash
git clone --recursive <repository_url>
```

## Switch Branch
```bash
git checkout <branch> 
```
or 
```bash
git switch <branch>
```


## Check Status
```
git status
```


## Pull
```
git pull
```

## Push

```bash
git add .
git commit -m "update"
git push origin main
```

By default, ```git push``` is using ```git push --no-rebase```. See following:

| **Command** | **Meaning** |
|----------|----------|
| ```git pull --no-rebase``` | Merges remote changes into your local branch. Creates a merge commit. |
| ```git pull --rebase``` | Applied your local commits on top of remote commits. |




