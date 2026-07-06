# Docker Commands Reference Guide

A quick reference for common Docker commands used in development and debugging.

---

## Table of Contents
1. [Image Management](#image-management)
2. [Container Lifecycle](#container-lifecycle)
3. [Running Containers](#running-containers)
4. [Interactive Access](#interactive-access)
5. [Container Session Management](#container-session-management)
6. [Inspection & Debugging](#inspection--debugging)
7. [Cleanup Commands](#cleanup-commands)
8. [Common Workflows](#common-workflows)

---

## Image Management

### Build an Image
```bash
docker build -t <image-name> .
docker build -t base-image-test .
```

### View Image History (layers)
```bash
docker history <image_id>
```

### List Images
```bash
docker images
docker images | grep <search-term>
```

### Remove an Image
```bash
docker rmi <image_name_or_id>
```
> ⚠️ Cannot remove images with running/stopped containers. Remove containers first.

---

## Container Lifecycle

### Start a Stopped Container
```bash
docker start <container_id_or_name>
docker start 3ad4a4aaafe0
```

### Stop a Running Container
```bash
docker stop <container_id_or_name>
docker stop f68efe5e529f
```

### List Containers
```bash
docker ps              # Running containers only
docker ps -a           # All containers (including stopped)
```

### Remove a Stopped Container
```bash
docker rm <container_id_or_name>
```

### Remove All Stopped Containers
```bash
docker container prune  #don't use on shared instance
```

---

## Running Containers

### Basic Run
```bash
docker run <image-name>
```

### Run and Auto-Remove After Exit
```bash
docker run --rm <image-name>
```
> `--rm` automatically removes container when it exits. Use for one-off commands.

### Run Interactively
```bash
docker run -it <image-name>
docker run -it rfm-base-image-test
```
> `-i` = interactive (keep STDIN open), `-t` = allocate pseudo-TTY

### Run with Shell (override ENTRYPOINT)
```bash
docker run -it <image-name> sh
docker run --rm -it --entrypoint /bin/bash <image-name>
```

### Run Specific Command (override ENTRYPOINT)
```bash
# Check Python version
docker run --rm --entrypoint python <image-name> --version

# Export installed packages
docker run --rm --entrypoint pip <image-name> freeze > requirements-pinned.txt

# Run Python script
docker run --rm --entrypoint python <image-name> /app/test.py
```

### Run with Environment Variables
```bash
docker run -e ENV=dev -e AWS_REGION=us-west-2 <image-name>
```

### Run with Resource Limits
```bash
docker run --memory=8g --cpus=4 <image-name>
```

---

## Interactive Access

### Access a Running Container
```bash
docker exec -it <container_id_or_name> sh
docker exec -it <container_id_or_name> bash
docker exec -it 3ad4a4aaafe0 sh
```

### Run Commands Inside Container
```bash
# Single command
docker exec <container> python --version

# Multiple commands
docker exec -it <container> bash -c "
    mkdir -p /app/data/05_reports
    python -c \"from entrypoint.b1 import upload_results_to_s3; upload_results_to_s3('bucket', 'prefix', 'region')\"
"
```

### Exit Container Shell
```bash
exit
# or
exit()
# or press Ctrl+D
```

---

## Container Session Management

### Detach Without Stopping Container
Press: `Ctrl + P` then `Ctrl + Q`

> This exits the terminal but keeps the container running in the background.

### Re-attach to Container Session
```bash
docker attach <container_id_or_name>
```
> Returns to the same shell session you detached from.

### Difference: `attach` vs `exec`

| Command | Behavior |
|---------|----------|
| `docker attach` | Connects to the main process (PID 1). If you exit, container stops. |
| `docker exec` | Spawns new process inside container. Exiting doesn't stop container. |

**Recommendation**: Use `exec` for most debugging scenarios.

---

## Inspection & Debugging

### View Container Logs
```bash
docker logs <container_id>
docker logs -f <container_id>      # Follow (tail -f)
docker logs --tail 100 <container> # Last 100 lines
```

### Inspect Container Details
```bash
docker inspect <container_id>
docker inspect --format '{{.State.Status}}' <container_id>
```

### Check Environment Variables Inside Container
```bash
docker exec <container> env
docker exec <container> env | grep ENV
```

### Copy Files To/From Container
```bash
# Copy from container to host
docker cp <container>:/app/output.csv ./output.csv

# Copy from host to container
docker cp ./local_file.txt <container>:/app/
```

---

## Cleanup Commands

### Remove Stopped Containers
```bash
docker container prune
```

### Remove Unused Images
```bash
docker image prune          # Dangling images only
docker image prune -a       # All unused images
```

### Remove Everything Unused
```bash
docker system prune         # Containers, networks, dangling images
docker system prune -a      # Also removes unused images
```
> ⚠️ Use with caution in shared environments.

### Check Disk Usage
```bash
docker system df
```

---

## Common Workflows

### Workflow 1: Debug a Failed Container
```bash
# 1. Run interactively with shell
docker run --rm -it --entrypoint /bin/bash <image-name>

# 2. Inside container, run your commands manually
python inference.py

# 3. Check environment
env | grep ENV
```

### Workflow 2: Test Image Without Running Pipeline
```bash
# Check Python version
docker run --rm --entrypoint python <image-name> --version

# Check installed packages
docker run --rm --entrypoint pip <image-name> list

# Run smoke test
docker run --rm --entrypoint python <image-name> /app/test.py
```

### Workflow 3: Run Pipeline Locally with Dev Config
```bash
docker run --rm \
    -e ENV=dev \
    -e AWS_REGION=us-west-2 \
    -e SECRET_NAME=axf-secrets-dev \
    <image-name>
```

### Workflow 4: Keep Container Running for Debugging
```bash
# Start container with infinite sleep
docker run -d --name debug-container <image-name> sleep infinity

# Exec into it anytime
docker exec -it debug-container bash

# When done, clean up
docker stop debug-container
docker rm debug-container
```

### Workflow 5: Export Requirements from Image
```bash
docker run --rm --entrypoint pip <image-name> freeze > requirements-pinned.txt
```

---

## Quick Reference Table

| Task | Command |
|------|---------|
| Build image | `docker build -t <name> .` |
| Run container | `docker run <image>` |
| Run interactively | `docker run -it <image>` |
| Run with auto-cleanup | `docker run --rm <image>` |
| Override entrypoint | `docker run --entrypoint <cmd> <image>` |
| Access running container | `docker exec -it <container> bash` |
| View logs | `docker logs <container>` |
| Stop container | `docker stop <container>` |
| Remove container | `docker rm <container>` |
| Remove image | `docker rmi <image>` |
| List running | `docker ps` |
| List all | `docker ps -a` |
| Detach without stop | `Ctrl+P`, `Ctrl+Q` |
| Re-attach | `docker attach <container>` |

---

## Tips

1. **Always use `--rm`** for one-off commands to avoid accumulating stopped containers
2. **Use `exec` instead of `attach`** for debugging - safer and more flexible
3. **Name your containers** with `--name` for easier reference: `docker run --name my-test ...`
4. **Use `-d` for detached mode** when you want container to run in background
5. **Check logs first** when debugging: `docker logs <container>` 
