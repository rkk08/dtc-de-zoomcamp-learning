## II. Docker Data Persistence: Volumes & Filesystem Access

### Problem
Containers are ephemeral by default.  
Any files created inside a container are lost when the container exits unless persistence is explicitly configured.

---

### Host File Operations
Basic file creation and inspection on the host:

```bash
touch file1.txt file2.txt file3.txt
echo "Hello from host" > file1.txt
ls
cat file1.txt
```

Notes:
- `echo` prints to stdout
- `>` writes or overwrites a file
- `>>` appends to a file
- Empty files remain empty until written to
     
---

### Accessing Files from a Container

To read or modify host files inside Docker, files must be shared via volumes.

A Python script can be used to inspect files inside a mounted directory.

---

### Python Filesystem Access (pathlib)

Using `pathlib` for directory traversal and file reading:

```python
from pathlib import Path

current_dir = Path.cwd()
current_file = Path(__file__).name

for filepath in current_dir.iterdir():
    if filepath.name == current_file:
        continue
    if filepath.is_file():
        print(filepath.name, filepath.read_text(encoding="utf-8"))
```

Key concepts:
- `pathlib` provides object-oriented filesystem handling
- `Path.cwd()` returns the current working directory
- `__file__` refers to the running script
- `.iterdir()` iterates over directory contents
- `.is_file()` filters out directories
- `==` compares values, = assigns values
- `continue` skips the current loop iteration

---

### Docker Volumes (Persistence Mechanism)

Volumes allow a container to access and persist data stored on the host filesystem.

Canonical command:

```bash

docker run -it \
  --rm \
  -v $(pwd):/app \
  --entrypoint=bash \
  python:3.13.11-slim
```

Explanation:
    `-it` runs the container interactively with a terminal
    `--rm` removes the container after exit
    `-v $(pwd):/app` mounts the current host directory into /app
    `--entrypoint=bash` starts a shell instead of Python
    `python:3.13.11-slim` is a minimal Python image

---

### Host ↔ Container Relationship

    - Host directory is mounted into /app inside the container
    - Files created or modified in /app persist on the host
    - Files outside mounted paths remain isolated inside the container

---

### Verification Inside Container

```bash

cd /app/test
ls
cat file1.txt
python list_files.py
```

Results confirm:

    - Host files are accessible inside the container
    - File contents persist across container runs

---

### Key Takeaways

- Containers do not persist data by default
- Docker volumes enable filesystem persistence
- `pathlib` is preferred over `os.path`
- Entering the container shell is expected behavior
- This pattern is foundational for reproducible DE workflows

---

### TL;DR

Use Docker volumes to run Python in isolated containers while preserving files on the host system.

