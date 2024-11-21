[return to table](../README.md)

---


### Delete a Conda Environment
```
conda remove --name env_name --all
```


### Clone (make a copy of) a Conda Environment
```
conda create --name new_env --clone existing_env
```

### Write Conda Environment Details to a .yml File
```
conda env export > environment.yml
```

### Create from .yml
```
conda env create -f environment.yml
```


## Conda-Pack

[**Conda-Pack**](https://conda.github.io/conda-pack/) is a tool that creates a portable archive of your Conda environment, allowing you to move it to another system with the same operating system and architecture.

### Step 1: Install conda-pack
```
conda install -c conda-forge conda-pack
```

### Step 2: Pack the environment
```
conda pack -n your_environment_name -o myenv.tar.gz
```

### Step 3: Unpack on new computer
```
mkdir -p /path/to/myenv
tar -xzf myenv.tar.gz -C /path/to/myenv
```

### Step 4: Fix paths
```
# Activate the environment
source /path/to/myenv/bin/activate

# Run conda-unpack
conda-unpack
```


