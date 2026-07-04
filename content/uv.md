
`uv` can be used in two ways: as a **full project manager** (like Poetry/Pipenv) or as a **drop-in replacement for `pip` and `venv`**. 

## 1. The Modern Way (Project Management)
This is the recommended workflow. `uv` automatically creates and manages the virtual environment for you—you don't even need to activate it manually.

| Task | Command | Description |
| :--- | :--- | :--- |
| **Initialize project** | `uv init` | Creates a `pyproject.toml` and basic project structure. |
| **Add a package** | `uv add requests` | Adds dependency to `pyproject.toml`, updates lockfile, and installs it into the auto-managed `.venv`. |
| **Remove a package** | `uv remove requests` | Removes the dependency and uninstalls it. |
| **Run a script** | `uv run main.py` | Runs the script inside the virtual environment automatically (creates the venv if it doesn't exist). |
| **Run a tool** | `uvx ruff check` | Fetches and runs a tool (like `ruff` or `pytest`) in an isolated ephemeral environment. |

---

## 2. The Traditional Way (Drop-in `pip` Replacement)
If you prefer the classic workflow of manually creating a virtual environment and installing packages via a `requirements.txt`, use the `uv pip` interface.

### Creating and Activating a Virtual Environment

```bash
# Create a virtual environment (defaults to .venv)
uv venv

# Create a venv with a specific Python version
uv venv --python 3.12

# Activate it (Mac/Linux)
source .venv/bin/activate

# Activate it (Windows)
.venv\Scripts\activate
```

### Installing Packages
*Note: You must activate the virtual environment first, or explicitly pass `--system` (not recommended).*

```bash
# Install a single package
uv pip install requests

# Install from a requirements file
uv pip install -r requirements.txt

# Uninstall a package
uv pip uninstall requests

# List installed packages
uv pip list
```

### Dependency Management (Replacing `pip-tools`)
If you use `requirements.in` to lock dependencies into a `requirements.txt`:

```bash
# Compile requirements.in into a locked requirements.txt
uv pip compile requirements.in -o requirements.txt

# Sync your virtual environment to exactly match the requirements.txt
uv pip sync requirements.txt
```
