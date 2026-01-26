# I. Docker Fundamentals: Functionality & Environment

## What is Docker?
- Docker runs apps in isolated containers
- Anything done inside a container does NOT affect your host machine
- Containers are ephemeral: exiting or removing them clears the environment unless volumes are used

## Key Commands & Concepts
- `docker run -it <image>` → Start a container interactively with a terminal
  - `-i` → interactive (keeps stdin open)
  - `-t` → terminal (allocates a pseudo-terminal)
- `docker ps` → Shows currently running containers
- `docker ps -a` → Shows all containers (running + stopped)
- `docker ps -aq` → Lists only container IDs
- `docker rm \`docker ps -aq\`` → Deletes all containers (be careful!)
- `docker run -it --entrypoint=bash <image>` → Opens bash in the container

## Images & Variants
- Example: `python:3.13.11-slim`
  - `3.13.11` → Python version pinned for reproducibility
  - `slim` → minimal, smaller image, faster download, less disk space
- Variant order matters: always `python:<version>-<variant>`

## Python in Docker
- `python3 -V` → Check Python version inside container
- Python REPL (`>>>`) is separate from bash terminal
- Ctrl + D or `exit()` → Exit Python REPL

## Notes on Persistence
- Containers are temporary
- Exiting a container resets its filesystem unless volumes are mounted
- Clean up old containers with:
  ```bash
  docker ps -aq       # get all container IDs
  docker rm $(docker ps -aq)  # remove all containers
