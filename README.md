# Docker and Containerization Overview

## What is Docker?

Docker is an open-source platform that enables developers to build, deploy, and run applications inside containers. A container is a lightweight, standalone, executable package that includes everything needed to run an application: code, runtime, system tools, libraries, and settings. Docker uses containerization technology to ensure applications run consistently across different environments, from development to production. It leverages Linux kernel features like cgroups and namespaces to provide isolation and resource management, making it portable across Linux, Windows, and macOS systems.

## Why Docker?

Docker simplifies and accelerates the development, deployment, and management of applications by offering the following benefits:

- **Consistency**: Containers ensure applications run identically across development, testing, and production environments, reducing "it works on my machine" issues.
- **Portability**: Docker containers can run on any system with Docker installed, regardless of the underlying OS or infrastructure.
- **Efficiency**: Containers share the host OS kernel, making them lightweight compared to virtual machines, with faster startup times and lower resource usage.
- **Scalability**: Docker supports microservices architectures, allowing individual components to scale independently.
- **Ecosystem**: Docker Hub provides a vast repository of pre-built images, and tools like Docker Compose and Docker Swarm enhance multi-container management and orchestration.

Docker is widely used in CI/CD pipelines, cloud deployments, and microservices due to its ease of use and robust ecosystem.

## What is Virtualization?

Virtualization is a technology that creates virtual representations of physical computing resources, such as servers, storage, or networks, allowing multiple virtual environments to run on a single physical machine. It relies on a **hypervisor** (e.g., VMware ESXi, Microsoft Hyper-V, VirtualBox, KVM) to abstract hardware resources and emulate complete virtual machines (VMs). Each VM includes a full guest operating system, virtualized hardware (CPU, memory, storage, network), and the application stack, enabling multiple OSes to run concurrently on the same host.

### In-Depth Explanation of Virtualization

- **Types of Virtualization**:
  - **Full Virtualization**: The hypervisor emulates the entire hardware stack, allowing unmodified guest OSes to run (e.g., VMware Workstation). This provides strong isolation but incurs higher overhead.
  - **Para-Virtualization**: The guest OS is modified to interact directly with the hypervisor, improving performance but requiring OS changes (e.g., Xen with para-virtualized guests).
  - **Hardware-Assisted Virtualization**: Leverages CPU features (e.g., Intel VT-x, AMD-V) to improve performance and allow unmodified OSes to run efficiently.
  
- **How It Works**:
  1. The hypervisor (Type 1: bare-metal, like ESXi, or Type 2: hosted, like VirtualBox) runs on the physical host.
  2. Each VM is allocated dedicated resources (vCPU, RAM, disk) and runs a complete OS kernel, isolated from other VMs.
  3. The hypervisor manages resource allocation, scheduling, and isolation, translating VM requests to physical hardware.
  4. VMs communicate with the host and external networks via virtual network interfaces.

- **Advantages**:
  - **Strong Isolation**: Each VM has its own kernel, ensuring robust security and fault isolation.
  - **OS Flexibility**: Supports diverse OSes (e.g., Windows, Linux, FreeBSD) on the same host.
  - **Legacy Support**: Ideal for running legacy applications requiring specific OS versions.
  
- **Challenges**:
  - **Resource Overhead**: VMs require significant CPU, memory, and storage due to full OS instances.
  - **Slow Startup**: Booting a full OS takes seconds to minutes.
  - **Large Footprint**: VM images are often several GBs, impacting storage and portability.

### Virtualization Flowchart

Below is a flowchart illustrating the architecture of virtualization, showing how the hypervisor manages multiple VMs on a physical host.

```
+---------------------------------------------------+
|               Physical Hardware                   |
|         (CPU, RAM, Disk, Network, etc.)           |
+--------------------------+------------------------+
                           |
                  +--------v--------+                      
                  |    Hypervisor    |   <- Type 1 or 2    
                  +--------+--------+                      
                           |
       +-------------------+-------------------+
       |                   |                   |
+------v------+     +------v------+     +------v------+
|  VM 1       |     |  VM 2       |     |  VM 3       |
|------------ |     |------------ |     |------------ |
| Guest OS    |     | Guest OS    |     | Guest OS    |
| App + Libs  |     | App + Libs  |     | App + Libs  |
+-------------+     +-------------+     +-------------+
```

**Explanation**: The physical hardware hosts a hypervisor, which creates and manages multiple VMs, each with its own guest OS and virtualized resources. For Type 2 hypervisors, a host OS sits between the hardware and hypervisor.

## What is Containerization?

Containerization is a lightweight form of virtualization that allows applications to run in isolated environments called containers. Containers package an application with its dependencies (libraries, binaries, configurations) but share the host operating system’s kernel. This eliminates the need for a full guest OS, making containers smaller, faster, and more resource-efficient than VMs. Docker, introduced in 2013, popularized containerization by providing a user-friendly platform for building, sharing, and running containers.

### In-Depth Explanation of Containerization

- **How It Works**:
  1. Containers use Linux kernel features like **namespaces** (for isolating processes, network, filesystem) and **cgroups** (for resource limiting) to create isolated environments.
  2. A container image is a lightweight, portable snapshot of an application and its dependencies, built from a **Dockerfile** or similar configuration.
  3. The container runtime (e.g., Docker, containerd) manages container lifecycle: creation, execution, and termination.
  4. Containers run as isolated processes on the host, sharing the kernel but with separate filesystems and resources.

- **Components**:
  - **Container Image**: A read-only template containing the application, libraries, and dependencies (e.g., an Nginx image).
  - **Container**: A running instance of an image, created by the runtime.
  - **Registry**: A repository (e.g., Docker Hub) for storing and distributing images.
  - **Runtime**: Software (e.g., Docker Engine, containerd) that executes containers.

- **Advantages**:
  - **Lightweight**: Containers share the host kernel, using minimal resources (MBs vs. GBs for VMs).
  - **Fast Startup**: Containers start in milliseconds, as no OS boot is required.
  - **Portability**: Images run consistently across environments with a compatible runtime.
  - **Scalability**: Containers are ideal for microservices, enabling rapid scaling and deployment.

- **Challenges**:
  - **Weaker Isolation**: Shared kernel means potential security risks if not properly configured.
  - **OS Limitation**: Containers typically require a compatible kernel (e.g., Linux containers on Linux hosts).
  - **Complexity**: Managing many containers requires orchestration tools like Docker Swarm or Kubernetes.

### Containerization Flowchart

Below is a flowchart depicting the containerization process, from building an image to running a container.

```
+---------------------------------------------------+
|               Physical Hardware                   |
|         (CPU, RAM, Disk, Network, etc.)           |
+--------------------------+------------------------+
                           |
               +-----------v-----------+ 
               |     Host OS Kernel    |  <- Shared by all containers
               | (Namespaces, cgroups) |
               +-----------+-----------+
                           |
       +-------------------+-------------------+
       |                   |                   |
+------v------+     +------v------+     +------v------+
| Container 1 |     | Container 2 |     | Container 3 |
|------------ |     |------------ |     |------------ |
| App + Libs  |     | App + Libs  |     | App + Libs  |
+-------------+     +-------------+     +-------------+
```

**Explanation**: A Dockerfile defines the application and dependencies, which are built into a container image. The image can be stored in a registry like Docker Hub or used directly to run containers. Containers execute as isolated processes on the host OS, sharing the kernel.

## Virtualization vs. Containerization

| **Aspect**                | **Virtualization**                                      | **Containerization**                                   |
|---------------------------|-------------------------------------------------------|------------------------------------------------------|
| **Architecture**          | Emulates a full physical machine with a guest OS.      | Shares host OS kernel, includes only app dependencies. |
| **Resource Usage**        | High (full OS per VM, more CPU/RAM/disk).             | Low (lightweight, minimal overhead).                  |
| **Startup Time**          | Slow (boots entire OS).                               | Fast (near-instant startup).                         |
| **Isolation**             | Strong (separate kernels, high security).              | Moderate (shared kernel, relies on namespaces/cgroups). |
| **Portability**           | Moderate (VM images are large, may need conversion).   | High (lightweight images, consistent across platforms). |
| **Use Case**              | Running multiple OSes or legacy apps.                  | Microservices, CI/CD, scalable cloud apps.            |
| **Performance**           | Lower due to hypervisor and guest OS overhead.         | Near-native due to shared kernel and minimal layers.  |
| **Storage**               | VMs require GBs per instance (full OS + app).          | Containers require MBs (app + dependencies only).     |
| **Flexibility**           | Supports diverse OSes (Windows, Linux, etc.).          | Limited to compatible kernels (e.g., Linux containers). |

**When to Use**:
- **Virtualization**: Ideal for applications requiring full OS isolation, different OS types, or legacy monolithic apps (e.g., running Windows Server on a Linux host).
- **Containerization**: Best for microservices, rapid deployment, CI/CD pipelines, and environments needing portability and efficiency.

## Alternatives to Docker

While Docker is the leading containerization platform, several alternatives offer similar or complementary functionality, addressing different use cases or preferences:

1. **Podman**:
   - A daemonless, open-source container engine developed by Red Hat.
   - Fully compatible with OCI standards and Docker CLI (e.g., `podman run` mimics `docker run`).
   - Supports rootless containers, enhancing security by not requiring root privileges.
   - Integrates with Buildah for image creation and Podman Compose for multi-container apps.
   - Ideal for security-focused environments or Kubernetes integration.

2. **LXC (Linux Containers)**:
   - An OS-level virtualization technology for running multiple isolated Linux systems.
   - Containers behave like lightweight VMs, including a full OS, suitable for complex workloads.
   - Offers near-native performance and direct hardware access but requires deeper Linux expertise.
   - Best for scenarios needing full OS functionality or VM-like persistence.

3. **containerd**:
   - A lightweight container runtime, originally part of Docker, now used by Kubernetes.
   - Focuses on simplicity and efficiency, supporting OCI-compliant images.
   - Ideal for Kubernetes-centric environments or minimalistic setups.

4. **CRI-O**:
   - A container runtime designed specifically for Kubernetes, emphasizing simplicity and performance.
   - Compatible with OCI images and integrates seamlessly with Kubernetes clusters.
   - Suitable for cloud-native, Kubernetes-focused deployments.

5. **Buildah**:
   - A tool for building OCI-compliant container images without a daemon.
   - Works with Podman for a complete container management solution.
   - Best for CI/CD pipelines needing customized image builds.

Each alternative has unique strengths, such as Podman’s security focus or LXC’s VM-like capabilities, making the choice dependent on project needs like security, ecosystem maturity, or orchestration requirements.

## Docker Swarm

Docker Swarm is Docker’s native container orchestration tool, built into the Docker Engine, for managing clusters of Docker containers across multiple nodes. It operates in "swarm mode," where nodes are either managers (handling cluster state) or workers (running tasks). Swarm provides:

- **Features**:
  - Simple setup and integration with Docker CLI and Compose.
  - Overlay networking for secure container communication.
  - Basic scaling and load balancing.
  - Fault tolerance via task rescheduling.
- **Use Case**: Ideal for small to medium-sized applications or teams already using Docker, needing lightweight orchestration without Kubernetes’ complexity.
- **Limitations**: Less feature-rich than Kubernetes, with limited automation and customization for large-scale deployments.

## Docker Compose

Docker Compose is a tool for defining and managing multi-container Docker applications using a YAML file. It simplifies local development and testing by allowing developers to configure services, networks, and volumes in a single file.

- **Features**:
  - Define multiple containers, their dependencies, and configurations in a `docker-compose.yml` file.
  - Commands like `docker-compose up` start all services, and `docker-compose down` stops them.
  - Useful for local development or small-scale deployments.
- **Limitations**: Not designed for large-scale orchestration; use Swarm or Kubernetes for production clusters.

### Example Docker Compose File
```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
```

## Amazon ECS (Elastic Container Service)

Amazon ECS is a fully managed container orchestration service provided by AWS. It simplifies deploying, scaling, and managing containerized applications.

- **Features**:
  - Integrates with AWS services like ECR (Elastic Container Registry), IAM, and VPC.
  - Supports Docker containers and Fargate (serverless) for running containers without managing servers.
  - Offers task definitions (similar to Docker Compose) and services for scaling and load balancing.
- **Use Case**: Ideal for AWS-based deployments needing managed orchestration and scalability.
- **Comparison**: Simpler than Kubernetes but less customizable; tightly integrated with AWS ecosystem.

## LXC Container

LXC (Linux Containers) is a low-level containerization technology that uses Linux kernel features (cgroups, namespaces) to run multiple isolated Linux systems on a single host. Unlike Docker, LXC containers are system containers, behaving like lightweight VMs with a full OS.

- **Features**:
  - Near-native performance and direct hardware access.
  - Persistent containers, suitable for long-running workloads.
  - Supports complex, multi-process applications.
- **Use Case**: Best for data-intensive apps, virtual desktop infrastructure (VDI), or scenarios requiring full OS control.
- **Limitations**: Steeper learning curve and less user-friendly than Docker.

## Podman

Podman is a daemonless, open-source container engine developed by Red Hat, designed as a Docker alternative. It supports OCI-compliant containers and mimics Docker’s CLI for easy adoption.

- **Features**:
  - **Daemonless**: No central daemon, reducing attack surface and resource usage.
  - **Rootless**: Runs containers without root privileges, enhancing security.
  - **Podman Compose**: Mimics Docker Compose for multi-container apps.
  - **Kubernetes Integration**: Works seamlessly as a Kubernetes runtime.
- **Use Case**: Ideal for security-conscious environments, Kubernetes clusters, or systemd integration.
- **Limitations**: Smaller ecosystem than Docker; some Docker Compose features (e.g., Host Network Mode) may not be fully supported.

## Installing Docker

### Installing Docker on Amazon Linux Server

1. **Update packages**  
   ```sh
   yum update -y
   ```

2. **Install Docker**  
   ```sh
   yum install docker -y
   ```

### Installing Docker on CentOS Server
*Reference URL: https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository*

#### Prerequisites

1. **Install required packages**  
   ```sh
   sudo yum install -y yum-utils \
     device-mapper-persistent-data \
     lvm2
   ```

2. **Set up the stable repository**  
   ```sh
   sudo yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
   ```

#### Installing Docker CE

1. **Install the latest version of Docker CE**  
   ```sh
   sudo yum install docker-ce
   ```
   If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

2. **Start Docker**  
   ```sh
   sudo systemctl start docker
   ```

3. **Verify Docker installation**  
   Run the hello-world image to confirm Docker is installed correctly:  
   ```sh
   sudo docker run hello-world
   ```

## 🐳 Docker Commands Cheat Sheet

## 🔧 **1. Docker Installation & Setup**

| Command            | Description                           |
| ------------------ | ------------------------------------- |
| `docker --version` | Check Docker version                  |
| `docker info`      | Show system-wide Docker info          |
| `docker login`     | Log in to DockerHub or registry       |
| `docker logout`    | Log out of Docker registry            |
| `docker version`   | Shows client & server version details |

---

## 📦 **2. Docker Image Commands**

| Command                                | Description                                       |
| -------------------------------------- | ------------------------------------------------- |
| `docker pull <image>`                  | Download image from Docker Hub                    |
| `docker images`                        | List all images                                   |
| `docker rmi <image>`                   | Remove image                                      |
| `docker build -t <name:tag> .`         | Build image from Dockerfile                       |
| `docker tag <image> <new-name:tag>`    | Tag image for pushing                             |
| `docker push <image>`                  | Push image to DockerHub or registry               |
| `docker image inspect <image>`         | Show detailed metadata of image                   |
| `docker image prune`                   | Remove unused images                              |
| `docker save -o <file.tar> <image>`    | Save an image to a tar archive                    |
| `docker load -i <file.tar>`            | Load an image from a tar archive                  |
| `docker image ls --filter <key=value>` | Filter images by criteria (e.g., `dangling=true`) |

---

## 🏃 **3. Docker Container Commands**

| Command                                    | Description                                             |
| ------------------------------------------ | ------------------------------------------------------- |
| `docker run <image>`                       | Run container from image                                |
| `docker run -it <image> /bin/bash`         | Run interactively                                       |
| `docker run -d <image>`                    | Run in background (detached mode)                       |
| `docker run -p 8080:80 <image>`            | Map port 8080 (host) to 80 (container)                  |
| `docker run -v /host:/container <image>`   | Mount volume                                            |
| `docker start <container>`                 | Start existing container                                |
| `docker stop <container>`                  | Stop running container                                  |
| `docker restart <container>`               | Restart container                                       |
| `docker rm <container>`                    | Remove container                                        |
| `docker ps`                                | List running containers                                 |
| `docker ps -a`                             | List all containers (including stopped)                 |
| `docker exec -it <container> <cmd>`        | Run command inside container                            |
| `docker logs <container>`                  | Show logs of container                                  |
| `docker inspect <container>`               | Show metadata/config of container                       |
| `docker inspect <container>`               | Show metadata/config of container                       |
| `docker pause <container>`                 | Pause all processes in a container                      |
| `docker unpause <container>`               | Unpause all processes in a container                    |
| `docker rename <container> <new-name>`     | Rename a container                                      |
| `docker cp <container>:<path> <host-path>` | Copy files/folders from container to host               |
| `docker cp <host-path> <container>:<path>` | Copy files/folders from host to container               |
| `docker commit <container> <image-name>`   | Create a new image from a container’s changes           |
| `docker export -o <file.tar> <container>`  | Export a container’s filesystem as a tar archive        |
| `docker import <file.tar> <image-name>`    | Import a tar archive to create an image                 |
| `docker create <image>`                    | Create a new container but do not start it              |
| `docker kill <container>`                  | Forcefully stop a running container                     |
| `docker update <container>`                | Update configuration (e.g., CPU, memory) of a container |
| `docker wait <container>`                  | Block until container stops, then print its exit code   |

---

## 🧹 **4. Clean Up Commands**

| Command                          | Description                                      |
| -------------------------------- | ------------------------------------------------ |
| `docker rm $(docker ps -aq)`     | Remove all containers                            |
| `docker rmi $(docker images -q)` | Remove all images                                |
| `docker volume prune`            | Remove unused volumes                            |
| `docker system prune`            | Remove unused containers, networks, images       |
| `docker system prune -a`         | Remove all unused images, not just dangling ones |
| `docker system df`               | Show Docker disk usage                           |
| `docker builder prune`           | Remove unused build cache                        |
               
---

## 📁 **5. Docker Volume Commands**

| Command                        | Description            |
| ------------------------------ | ---------------------- |
| `docker volume create <name>`  | Create named volume    |
| `docker volume ls`             | List all volumes       |
| `docker volume inspect <name>` | Inspect volume details |
| `docker volume rm <name>`      | Remove volume          |

---

## 🌐 **6. Docker Network Commands**

| Command                                       | Description                       |
| --------------------------------------------- | --------------------------------- |
| `docker network ls`                           | List networks                     |
| `docker network create <name>`                | Create a new network              |
| `docker network inspect <name>`               | Details about a network           |
| `docker network rm <name>`                    | Remove a network                  |
| `docker network connect <net> <container>`    | Connect container to network      |
| `docker network disconnect <net> <container>` | Disconnect container from network |
| `docker network prune`                        | Remove all unused networks        |

---

## 🐋 **7. Docker Compose Commands** (for multi-container apps)

| Command                               | Description                                       |
| ------------------------------------- | ------------------------------------------------- |
| `docker-compose up`                   | Start services defined in `docker-compose.yml`    |
| `docker-compose up -d`                | Start in detached mode                            |
| `docker-compose down`                 | Stop and remove all containers                    |
| `docker-compose build`                | Build images from `docker-compose.yml`            |
| `docker-compose ps`                   | List containers started by Compose                |
| `docker-compose logs`                 | Show logs from Compose containers                 |
| `docker-compose restart`              | Restart services defined in docker-compose.yml    |
| `docker-compose stop`                 | Stop services without removing them               |
| `docker-compose rm`                   | Remove stopped service containers                 |
| `docker-compose pull`                 | Pull service images defined in docker-compose.yml |
| `docker-compose exec <service> <cmd>` | Run a command in a running service container      |

---

## 🧪 **8. Debugging & Troubleshooting**

| Command                              | Description                                                  |
| ------------------------------------ | ------------------------------------------------------------ |
| `docker logs <container>`            | View container logs                                          |
| `docker top <container>`             | Show running processes in container                          |
| `docker stats`                       | Real-time resource usage of containers                       |
| `docker events`                      | Real-time events from Docker daemon                          |
| `docker diff <container>`            | Inspect changes to files/folders in a container’s filesystem |
| `docker attach <container>`          | Attach to a running container’s terminal                     |
| `docker port <container>`            | List port mappings for a container                           |
| `docker events --filter <key=value>` | Filter real-time events from Docker daemon                   |

---

## 🧬 **9. Dockerfile Related**

| Command                                          | Description                                          |
| ------------------------------------------------ | ---------------------------------------------------- |
| `docker build -f <Dockerfile> -t <image-name> .` | Build image from custom Dockerfile                   |
| `docker history <image>`                         | Show layer history of image                          |
| `docker manifest create <manifest> <image>`      | Create a manifest list for multi-architecture images |
| `docker manifest inspect <manifest>`             | Display details of a manifest list                   |
| `docker manifest push <manifest>`                | Push a manifest list to a registry                   |

---

## 🔐 **10. Docker Registry**

| Command                          | Description                              |
| -------------------------------- | ---------------------------------------- |
| `docker login <registry>`        | Login to private registry                |
| `docker push <registry>/<image>` | Push image to private registry           |
| `docker pull <registry>/<image>` | Pull image from private registry         |
| `docker trust sign <image>`      | Sign an image to verify its authenticity |
| `docker trust inspect <image>`   | Display trust information for an image   |

---

## 🐳 **11. Docker Swarm Commands**

| Command                                     | Description                                         |
|---------------------------------------------|-----------------------------------------------------|
| `docker swarm init`                         | Initialize a Swarm                                  |
| `docker swarm join`                         | Join a node to a Swarm                              |
| `docker swarm leave`                        | Leave a Swarm                                       |
| `docker node ls`                            | List nodes in the Swarm                             |
| `docker node inspect <node>`                | Display detailed information about a node           |
| `docker service create`                     | Create a new service in the Swarm                   |
| `docker service ls`                         | List services in the Swarm                          |
| `docker service scale <service>=<replicas>` | Scale a service to the specified number of replicas |
| `docker service rm <service>`               | Remove a service from the Swarm                     |
| `docker service logs <service>`             | View logs for a Swarm service                       |

---

## 🔧 **12. Docker System Management Commands**

| Command                         | Description                                                      |
|---------------------------------|------------------------------------------------------------------|
| `docker system info`            | Display detailed system-wide information                         |
| `docker system events`          | Monitor real-time system events (alternative to `docker events`) |
| `docker system prune --volumes` | Remove unused volumes along with other unused objects            |

---

## 🔐 **13. Docker Config and Secret Commands**

| Command                              | Description                 |
|--------------------------------------|-----------------------------|
| `docker config create <name> <file>` | Create a config from a file |
| `docker config ls`                   | List all configs            |
| `docker config rm <name>`            | Remove a config             |
| `docker secret create <name> <file>` | Create a secret from a file |
| `docker secret ls`                   | List all secrets            |
| `docker secret rm <name>`            | Remove a secret             |

---

## 📄 Example Use Case: Run NGINX

```bash
docker pull nginx
docker run -d -p 8080:80 --name webserver nginx
docker logs webserver
docker exec -it webserver bash
docker stop webserver
docker rm webserver
```

## 🐳 Docker Commands Important Commands

1. **Search for a Docker image on hub.docker.com**  
   ```sh
   docker search httpd
   ```

2. **Download a Docker image from hub.docker.com**  
   ```sh
   docker image pull <image_name>:<image_version/tag>
   ```

3. **List Docker images on your local system**  
   ```sh
   docker image ls
   ```

4. **Create/run/start a Docker container from an image**  
   ```sh
   docker run -d --name <container_Name> <image_name>:<image_version/tag>
   ```
   `-d`: Run the container in the background (detached mode).

5. **Expose your application to the host server**  
   ```sh
   docker run -d -p <host_port>:<container_port> --name <container_Name> <image_name>:<image_version/tag>
   ```
   Example:  
   ```sh
   docker run -d --name httpd_server -p 8080:80 httpd:2.2
   ```

6. **List running containers**  
   ```sh
   docker ps
   ```

7. **List all containers (running, stopped, terminated, etc.)**  
   ```sh
   docker ps -a
   ```

8. **Run an OS-based container in interactive mode**  
   ```sh
   docker run -i -t --name centos_server centos:latest
   ```
   `-i`: Interactive mode  
   `-t`: Allocate a terminal

9. **Stop a container**  
   ```sh
   docker stop <container_id>
   ```

10. **Start a stopped container**  
    ```sh
    docker start <container_id>
    ```

11. **Remove a container**  
    ```sh
    docker rm <container_id>
    ```

12. **Log in to a running Docker container**  
    ```sh
    docker exec -it <container_Name> /bin/bash
    ```

## Docker Exec

Docker Exec is a command used to run a command inside a running Docker container. It allows interaction with a container’s environment, such as accessing a shell, executing scripts, or debugging processes, without needing to start a new container. This is particularly useful for troubleshooting, inspecting container states, or performing administrative tasks.

### In-Depth Explanation of Docker Exec

- **How It Works**:
  - The `docker exec` command interacts with a running container by executing a specified command inside its isolated environment.
  - It leverages the container’s runtime environment, using the same namespaces and cgroups as the container’s main process.
  - Supports interactive (`-i`) and terminal (`-t`) modes for shell access or one-off commands.

- **Common Use Cases**:
  - Accessing a shell to inspect files or configurations (e.g., `bash` or `sh`).
  - Running diagnostic commands (e.g., `ps`, `top`, `env`).
  - Executing scripts or updating configurations inside a container.

- **Options**:
  - `-i`: Interactive mode, keeps STDIN open.
  - `-t`: Allocates a pseudo-TTY for terminal access.
  - `--user <username>`: Runs the command as a specific user.
  - `--env <VAR=value>`: Sets environment variables for the command.

### Example: Using Docker Exec
```bash
# Start an Nginx container
docker run -d --name webserver nginx:latest

# Access a bash shell inside the container
docker exec -it webserver bash

# Run a single command (e.g., list files)
docker exec webserver ls /usr/share/nginx/html

# Check running processes
docker exec webserver ps aux
```

**Explanation**: The `-it` flags enable interactive terminal access. The first example opens a shell inside the `webserver` container, while the second runs `ls` to list files in the Nginx web directory.

## Docker Volumes

Docker Volumes provide a way to persist data generated by containers, which are ephemeral by default (data is lost when a container is removed). Volumes are managed by Docker and stored outside the container’s filesystem, allowing data to be shared between containers or preserved across container lifecycles.

### In-Depth Explanation of Docker Volumes

- **How It Works**:
  - Volumes are created as separate storage entities managed by Docker, typically stored in `/var/lib/docker/volumes/` on the host.
  - They can be mounted into containers at specific paths, enabling data persistence and sharing.
  - Unlike bind mounts (which map host directories), volumes are isolated from the host filesystem, improving portability and security.

- **Types of Volumes**:
  - **Named Volumes**: Managed by Docker with a specific name, reusable across containers.
  - **Anonymous Volumes**: Automatically created with a random ID, tied to a single container.
  - **Bind Mounts**: Map a specific host directory to a container path (less portable).

- **Advantages**:
  - **Persistence**: Data survives container removal.
  - **Portability**: Named volumes work across different hosts.
  - **Performance**: Optimized for Docker’s storage drivers, often faster than bind mounts.
  - **Backup**: Volumes can be backed up or migrated easily.

- **Challenges**:
  - Managing volume lifecycle (e.g., cleaning up unused volumes).
  - Requires understanding of Docker’s storage drivers (e.g., overlay2).

### Example: Using Docker Volumes
```bash
# Create a named volume
docker volume create mydata

# Run a container with the volume mounted
docker run -d --name mysql_db -v mydata:/var/lib/mysql mysql:5.7

# Inspect the volume
docker volume inspect mydata

# Run another container to reuse the volume
docker run -d --name mysql_db2 -v mydata:/var/lib/mysql mysql:5.7

# Remove unused volumes
docker volume prune
```

**Explanation**: The `mydata` volume is created and mounted to `/var/lib/mysql` in a MySQL container, persisting the database data. The same volume can be reused in another container, ensuring data continuity.

## Docker Stats

Docker Stats is a command that displays real-time resource usage statistics for running containers, including CPU, memory, network I/O, and block I/O. It’s useful for monitoring container performance and diagnosing resource bottlenecks.

### In-Depth Explanation of Docker Stats

- **How It Works**:
  - `docker stats` queries the Docker daemon to retrieve resource usage metrics from the container’s cgroups.
  - It runs continuously, updating metrics in real time, unless interrupted or used with `--no-stream`.
  - Metrics include:
    - **CPU %**: Percentage of CPU usage relative to allocated resources.
    - **Mem Usage/Limit**: Current memory usage and allocated limit.
    - **Net I/O**: Network input/output in bytes.
    - **Block I/O**: Disk read/write in bytes.

- **Use Cases**:
  - Identifying containers consuming excessive resources.
  - Optimizing resource allocation for containers.
  - Debugging performance issues in production environments.

- **Options**:
  - `--no-stream`: Display a single snapshot instead of continuous updates.
  - `--format`: Customize output using Go templates (e.g., `{{.Name}} {{.CPUPerc}}`).

### Example: Using Docker Stats
```bash
# Start multiple containers
docker run -d --name web1 nginx:latest
docker run -d --name web2 nginx:latest

# View real-time stats for all containers
docker stats

# View stats for a specific container
docker stats web1

# Single snapshot with custom format
docker stats --no-stream --format "{{.Name}}: {{.CPUPerc}} {{.MemUsage}}"
```

**Explanation**: `docker stats` displays a live table of resource usage. The `--no-stream` option provides a one-time snapshot, useful for scripting or quick checks.

## Docker Hub Registry
*Reference URL: https://docs.docker.com/docker-hub/*

Docker Hub is a cloud-based registry service for linking to code repositories, building and testing images, storing manually pushed images, and deploying them to hosts. It provides centralized resources for container image discovery, distribution, change management, collaboration, and workflow automation.

### In-Depth Explanation of Pushing to Docker Registry

- **How It Works**:
  - Images are tagged with the registry name and repository (e.g., `username/image:tag`).
  - `docker login` authenticates with the registry using credentials.
  - `docker push` uploads the image layers to the registry, leveraging Docker’s layer caching for efficiency.

- **Prerequisites**:
  - A Docker Hub account (sign up at [https://hub.docker.com/](https://hub.docker.com/)).
  - A locally built or tagged image.
  - Network access to the registry.

- **Use Cases**:
  - Sharing images with team members or the public.
  - Deploying images to CI/CD pipelines or production environments.
  - Storing custom images for reuse across systems.

1. **Create a Docker Hub account**  
   Visit https://hub.docker.com/ to sign up.

2. **Pull a Docker image**  
   ```sh
   docker pull ubuntu
   ```

3. **Pull a Docker image with a specific version**  
   ```sh
   docker pull ubuntu:16.04
   ```

4. **Create a custom tag for a Docker image**  
   ```sh
   docker tag ubuntu:latest valaxy/ubuntu:demo
   ```

5. **Log in to Docker Hub registry**  
   ```sh
   docker login
   ```

6. **Push a custom image to Docker Hub**  
   ```sh
   docker push valaxy/ubuntu:demo
   ```

### Testing

1. **Remove all images from the Docker server**  
   ```sh
   docker image rm -f <Image_id>
   ```

2. **Pull your custom image from Docker Hub**  
   ```sh
   docker pull valaxy/ubuntu:demo
   ```

## Dockerfile

### What is a Dockerfile?

A Dockerfile is a text file containing instructions to build a Docker image. It defines the base image, environment, dependencies, and commands needed to create a reproducible container image.

#### Dockerfile Instructions

- **FROM**: Specifies the base image to download.
- **MAINTAINER**: Non-executable instruction indicating the author of the Dockerfile.
- **COPY**: Copies files from the Docker host to the image.
- **ADD**: Copies files from source to destination and can extract or download files from the internet.
- **CMD**: Specifies the default command for the image.
- **ENTRYPOINT**: Defines the starting point of execution when the container runs.
- **ENV**: Sets environment variables in the container.
- **EXPOSE**: Exposes a specified port.
- **RUN**: Executes a command on top of an existing layer during the build修理

System: build process.
- **USER**: Sets the username for the container.
- **VOLUME**: Enables access to a specified location.
- **WORKDIR**: Changes the working directory.
- **ONBUILD**: Adds a trigger instruction for later execution.

#### Difference Between ADD and COPY

- **COPY**: Copies files from the host to the image.
- **ADD**: Copies files and can also download files from the internet or extract archives.

#### Difference Between RUN and CMD

- **RUN**: Executes commands during the image build process, creating a new layer.
  Example:  
  ```sh
  RUN mkdir -p /path-to-folder
  ```
- **CMD**: Specifies the default command to run when the container starts. Only the last CMD is used if multiple are defined.

#### Difference Between CMD and ENTRYPOINT

- **CMD**: Can be overridden by passing arguments during `docker run`.
- **ENTRYPOINT**: Cannot be overridden and serves as the container’s executable command.
  
  Example:  
  ```dockerfile
  FROM ubuntu
  RUN apt-get update
  ENTRYPOINT ["echo", "hello"]
  CMD ["WORLD"]
  ```
  Running the container outputs: `hello WORLD`.  
  If an argument is passed (e.g., `docker run -it -d <image_name> Sabair`), the output becomes `hello Sabair`.

### Example: Single-Stage Dockerfile
*Clone the repository: https://github.com/betawins/multi-stage-example.git*

```dockerfile
FROM openjdk:8-jdk-alpine
RUN mkdir -p /app/source
COPY . /app/source
WORKDIR /app/source
RUN ./mvnw clean package

EXPOSE 8080
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app/source/target/multi-stage-example-0.0.1-SNAPSHOT.jar"]
```

- **Advantages**:
  - **Reproducibility**: Ensures consistent image builds.
  - **Automation**: Simplifies application packaging.
  - **Version Control**: Dockerfiles can be stored in Git for tracking changes.

- **Challenges**:
  - Optimizing layer caching to reduce build time.
  - Managing image size by minimizing unnecessary layers.

### Example: Multi-Stage Dockerfile

Multi-stage Dockerfiles reduce image size, improve security, and enable faster container startup.

```dockerfile
# Build Image
FROM openjdk:8-jdk-alpine as builder
RUN mkdir -p /app/source
COPY . /app/source
WORKDIR /app/source
RUN ./mvnw clean package

# Run Image
FROM openjdk:8-jdk-alpine
WORKDIR /app
COPY --from=builder /app/source/target/*.jar /app/app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app/app.jar"]
```

### Example: Tomcat Dockerfile

```dockerfile
FROM alpine:3.10
MAINTAINER Techie_Horizon
RUN mkdir /usr/local/tomcat/
WORKDIR /usr/local/tomcat
RUN apk --no-cache add curl && \
    apk add --update curl && \
    curl -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.73/bin/apache-tomcat-9.0.73.tar.gz
RUN tar -xvf apache*.tar.gz
RUN mv apache-tomcat-9.0.73/* /usr/local/tomcat/.
RUN rm -rf apache-*
COPY sample.war /usr/local/tomcat/webapps
RUN apk update && apk add openjdk8
WORKDIR /usr/local/tomcat
EXPOSE 8080
CMD ["/usr/local/tomcat/bin/catalina.sh", "run"]
```

## Docker Multistage File

Multistage Dockerfiles use multiple `FROM` statements to optimize image size and build process by separating the build environment from the runtime environment. This reduces the final image size, improves security, and enhances performance.

### In-Depth Explanation of Multistage Dockerfiles

- **How It Works**:
  - Each `FROM` statement starts a new build stage, often with a different base image.
  - Artifacts (e.g., compiled binaries) are copied from one stage to another using `COPY --from`.
  - The final stage contains only the runtime components, discarding unnecessary build tools.

- **Use Cases**:
  - Building applications (e.g., Java, Node.js) and including only the runtime artifacts.
  - Reducing image size by excluding build dependencies (e.g., compilers, SDKs).
  - Enhancing security by minimizing the attack surface in production images.

- **Advantages**:
  - **Smaller Images**: Excludes build tools from the final image.
  - **Cleaner Builds**: Separates build and runtime environments.
  - **Security**: Reduces vulnerabilities by limiting included software.

- **Challenges**:
  - Requires careful planning to copy only necessary artifacts.
  - Slightly more complex to write and debug.

### Example: Multistage Dockerfile
```dockerfile
# Build Stage
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Runtime Stage
FROM nginx:latest
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Explanation**: The first stage (`builder`) builds a Node.js application, while the second stage copies only the compiled assets to an Nginx image, resulting in a smaller, production-ready image.

## Docker Compose

### Sample Two-Tier Docker Compose File

```yaml
version: '3'
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db:330

System: 6
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data: {}
```

## Docker Save, Load, and Commit

Docker provides commands to manage images beyond building and pulling from registries. `docker save`, `docker load`, and `docker commit` are important for portability, backup, and customization.

---

### Docker Save

#### What is it?

`docker save` exports a Docker image, including all its layers and metadata, to a tar archive file.
This is useful for transporting images offline, archiving, or migrating images without using a Docker registry.

#### Key Features

* Saves image layers and metadata.
* Works completely offline.
* Can save multiple images if needed.

#### Syntax

```bash
docker save -o <output-tar-file> <image-name>:<tag>
```

#### Example

```bash
docker save -o myapp.tar myapp:latest
```

This creates a file `myapp.tar` containing the `myapp:latest` image.

#### Use Cases

* Moving images to air-gapped servers.
* Archiving stable image versions.
* Migrating images between Docker hosts without a registry.

---

### Docker Load

#### What is it?

`docker load` imports a Docker image from a tar archive created by `docker save`.
This reconstructs the image with its layers and tags.

#### Syntax

```bash
docker load -i <input-tar-file>
```

#### Example

```bash
docker load -i myapp.tar
```

This loads the image into the local Docker daemon. Docker reports which tags were loaded.

#### Use Cases

* Restoring image backups.
* Deploying pre-built images on servers without internet access.

---

### Docker Commit

#### What is it?

`docker commit` creates a new image from a container’s current state.
This captures the filesystem changes inside a running or stopped container and saves them as a new image.

Unlike `docker build`, which relies on a Dockerfile for reproducibility, `docker commit` snapshots the state directly.

#### How it Works

* Captures the current state of a container’s filesystem.
* Creates a new image layer from it.
* Tags the new image.

#### Syntax

```bash
docker commit [OPTIONS] <container-id> <new-image-name>:<tag>
```

Common options:

* `-a "Author Name"` — set the author
* `-m "Commit message"` — set a commit message

#### Example

```bash
## Start a container and install additional software
docker run -it ubuntu bash
apt update && apt install -y curl

## Exit the container, then find its ID
docker ps -a

## Commit the container state to a new image
docker commit -a "Alice" -m "Added curl" <container-id> ubuntu-with-curl:latest

## Verify the new image exists
docker images
```

#### Use Cases

* Quickly prototype changes without writing a Dockerfile.
* Capture manual debugging fixes from an interactive session.
* Create customized images in environments where CI pipelines or build systems are unavailable.

---

### Summary Table

| Command         | Purpose                           | Typical Use Cases                                |
| --------------- | --------------------------------- | ------------------------------------------------ |
| `docker save`   | Export image to tar archive       | Backups, offline sharing, air-gapped systems     |
| `docker load`   | Import image from tar archive     | Restore images, offline deployment               |
| `docker commit` | Create image from container state | Manual edits, prototyping, capturing debug state |

---

## Docker Scan

Docker Scan is a command-line tool integrated with Docker Desktop to scan Docker images for known vulnerabilities in software dependencies. It uses Snyk’s vulnerability database to identify security issues.

### In-Depth Explanation of Docker Scan

* **How It Works**:

  * `docker scan` analyzes image layers for vulnerabilities in OS packages and application dependencies (e.g., npm, pip).
  * It generates a report listing CVEs (Common Vulnerabilities and Exposures), severity levels, and remediation suggestions.
  * Requires Docker Desktop and a Snyk account (free tier available).

* **Use Cases**:

  * Ensuring images are secure before deployment.
  * Integrating security checks into CI/CD pipelines.
  * Identifying outdated or vulnerable dependencies.

* **Limitations**:

  * Requires Docker Desktop (not available on Linux servers without additional setup).
  * Limited to Snyk’s vulnerability database, which may not cover all packages.

### Example: Using Docker Scan

```bash
# Scan an image
docker scan nginx:latest

# Scan a specific tag
docker scan username/myapp:v1

# Output example
# Testing nginx:latest...
# Package: openssl
# Version: 1.1.1
# CVE: CVE-2022-1234
# Severity: High
# Fix: Upgrade to 1.1.2
```

**Explanation**: The command scans the `nginx:latest` image and outputs a report of vulnerabilities, including severity and suggested fixes.

---

## Docker Scout

Docker Scout is an integrated tool from Docker for analyzing container images, tracking vulnerabilities, generating SBOMs, and providing remediation advice, directly in the CLI, Docker Desktop, and Docker Hub.

### In-Depth Explanation of Docker Scout

* **How It Works**:

  * Analyzes image layers and dependencies to detect vulnerabilities (CVEs) and outdated packages.
  * Generates a Software Bill of Materials (SBOM) to list all components inside the image.
  * Compares images to show what changed (e.g., dev vs prod), and suggests updates for base images.

* **Use Cases**:

  * Proactively securing images by identifying known CVEs before deployment.
  * Auditing images to meet compliance standards.
  * Comparing image builds to understand impact on vulnerabilities.

* **Limitations**:

  * Requires Docker Desktop (for local analysis) or Docker Hub integration for cloud analysis.
  * Might need authentication to Docker Hub for private images.
  * Still evolving; not all features are available on Linux without Docker Desktop.

### Example: Using Docker Scout

```bash
# Quick vulnerability summary
docker scout quickview myapp:latest

# Detailed list of CVEs
docker scout cves myapp:latest

# Compare two images
docker scout compare myapp:v1 --to myapp:v2

# Generate an SBOM in SPDX format
docker scout sbom --format spdx -o myapp.sbom.json myapp:latest
```

**Explanation**: `docker scout` provides a rich set of tools for analyzing, comparing, and securing images, helping developers address vulnerabilities before production.

---

## Why your `docker scan` and `docker scout` commands are not working

**Reason:**

* `docker scan` is **not included by default on Linux** and was dropped from Docker Engine packages starting with Docker 23.x. It used to be bundled in Docker Desktop only.
* `docker scout` requires either Docker Desktop (macOS / Windows) or installing the Scout CLI separately on Linux.

---

**What we can do instead:**

* For scanning:

  * Use **Trivy** (`trivy image myapp:latest`)
  * Or **Snyk CLI** (`snyk container test myapp:latest`)
* For SBOM / CVE / compare features:

  * Install the [Docker Scout CLI standalone](https://docs.docker.com/scout/quickstart/) on Linux.
* Or continue using `docker sbom` (available since Docker 24+) to get the bill of materials.

---

### 🔹 1. **Dangling Images in Docker**

This is the most likely interpretation.

**Dangling images** are images that are **not tagged** and **not referenced** by any container. These are typically intermediate images created during builds.

#### 📌 Example

When you run:

```bash
docker images
```

You might see something like:

```
<none>    <none>    123abc456def    120MB
```

That’s a **dangling image**.

---

### 🔸 Why Dangling Images Exist?

Docker creates intermediate images during builds to optimize caching. If a build fails or images are rebuilt frequently, old layers can become untagged and unused.

---

### 🔧 How to List Dangling Images

```bash
docker images -f "dangling=true"
```

---

### 🧹 How to Remove Dangling Images

```bash
docker image prune
```

✅ To remove all unused images (not just dangling ones):

```bash
docker image prune -a
```

> ⚠️ Warning: Use `-a` with caution—it will remove **all unused** images, not just dangling ones.

---

### 🔍 Bonus: Check Disk Usage

To check how much space is used:

```bash
docker system df
```

---

### Summary Table

| Command                            | Purpose                    |
| ---------------------------------- | -------------------------- |
| `docker images -f "dangling=true"` | Show dangling images       |
| `docker image prune`               | Remove all dangling images |
| `docker image prune -a`            | Remove all unused images   |
| `docker system df`                 | Show Docker disk usage     |

---

## Understanding Docker Networking

*Created: Jully 12, 2025*  
*Saddam Hussain*

Docker is the de facto model for building and running containers at scale in most enterprise organizations today. At a very high level, Docker is a combination of CLI and a daemon process that solves common software problems like installing, publishing, removing, and managing containers. It’s perfect for microservices, where you have many services handling a typical business functionality; Docker makes the packaging easier, enabling you to encapsulate those services in containers.

Once the application is inside a container, it’s easier to scale and even runs on different cloud platforms, like AWS, GCP, and Azure. In this article, let’s focus on the networking aspect of Docker.

### What Is a Docker Network?

Networking is about communication among processes, and Docker’s networking is no different. Docker networking is primarily used to establish communication between Docker containers and the outside world via the host machine where the Docker daemon is running.

Docker supports different types of networks, each fit for certain use cases. We’ll be exploring the network drivers supported by Docker in general, along with some coding examples.

Docker networking differs from virtual machine (VM) or physical machine networking in a few ways:
1. Virtual machines are more flexible in some ways as they can support configurations like NAT and host networking. Docker typically uses a bridge network, and while it can support host networking, that option is only available on Linux.
2. When using Docker containers, network isolation is achieved using a network namespace, not an entirely separate networking stack.
3. You can run hundreds of containers on a single-node Docker host, so it’s required that the host can support networking at this scale. VMs usually don’t run into these network limits as they typically run fewer processes per VM.

### What Are Docker Network Drivers?

Docker handles communication between containers by creating a default bridge network, so you often don’t have to deal with networking and can instead focus on creating and running containers. This default bridge network works in most cases, but it’s not the only option you have.

Docker allows you to create three different types of network drivers out-of-the-box: bridge, host, and none. However, they may not fit every use case, so we’ll also explore user-defined networks such as overlay and macvlan. Let’s take a closer look at each one.

#### The Bridge Driver

This is the default. Whenever you start Docker, a bridge network gets created and all newly started containers will connect automatically to the default bridge network.

You can use this whenever you want your containers running in isolation to connect and communicate with each other. Since containers run in isolation, the bridge network solves the port conflict problem. Containers running in the same bridge network can communicate with each other, and Docker uses iptables on the host machine to prevent access outside of the bridge.

Let’s look at some examples of how a bridge network driver works.

1. Check the available network by running the `docker network ls` command.
2. Start two busybox containers named `busybox1` and `busybox2` in detached mode by passing the `-dit` flag.

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
5077a7b25ae6   bridge    bridge    local
7e25f334b07f   host      host      local
475e50be0fe0   none      null      local

$ docker run -dit --name busybox1 busybox /bin/sh
$ docker run -dit --name busybox2 busybox /bin/sh
```

3. Run the `docker ps` command to verify that containers are up and running.

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND     CREATED          STATUS          PORTS
9e6464e82c4c   busybox   "/bin/sh"   5 seconds ago    Up 5 seconds
7fea14032748   busybox   "/bin/sh"   26 seconds ago   Up 26 seconds
```

4. Verify that the containers are attached to the bridge network.

```bash
$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "5077a7b25ae67abd46cff0fde160303476c8a9e2e1d52ad01ba2b4bf",
        "Created": "2021-03-05T03:25:58.232446444Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "7fea140327487b57c3cf31d7502cfaf701e4ea4314621f0c726309e396": {
                "Name": "busybox1",
                "EndpointID": "05f216032784786c3315e30b3d54d50a25c1efc7",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "9e6464e82c4ca647b9fb60a85ca25e71370330982ea497d51c1238d073": {
                "Name": "busybox2",
                "EndpointID": "3dcc24e927246c44a2063b5be30b5f5e1787dcd4",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

5. Under the container’s key, you can observe that two containers (`busybox1` and `busybox2`) are listed with information about IP addresses. Since containers are running in the background, attach to the `busybox1` container and try to ping `busybox2` with its IP address.

```bash
$ docker attach busybox1
/ ## whoami
root
/ ## hostname -i
172.17.0.2
/ ## ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=2.083 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.144 ms
/ ## ping busybox2
ping: bad address 'busybox2'
```

6. Observe that the ping works by passing the IP address of `busybox2` but fails when the container name is passed instead.

The downside with the bridge driver is that it’s not recommended for production; the containers communicate via IP address instead of automatic service discovery to resolve an IP address to the container name. Every time you run a container, a different IP address gets assigned to it. It may work well for local development or CI/CD, but it’s definitely not a sustainable approach for applications running in production.

Another reason not to use it in production is that it will allow unrelated containers to communicate with each other, which could be a security risk. I’ll cover how you can create custom bridge networks later.

#### The Host Driver

As the name suggests, host drivers use the networking provided by the host machine. And it removes network isolation between the container and the host machine where Docker is running. For example, if you run a container that binds to port 80 and uses host networking, the container’s application is available on port 80 on the host’s IP address. You can use the host network if you don’t want to rely on Docker’s networking but instead rely on the host machine networking.

One limitation with the host driver is that it doesn’t work on Docker Desktop: you need a Linux host to use it. This article focuses on Docker Desktop, but I’ll show you the commands required to work with the Linux host.

The following command will start an Nginx image and listen to port 80 on the host machine:

```bash
$ docker run --rm -d --network host --name my_nginx nginx
```

You can access Nginx by hitting the `http://localhost:80/` URL.

The downside with the host network is that you can’t run multiple containers on the same host having the same port. Ports are shared by all containers on the host machine network.

#### The None Driver

The none network driver does not attach containers to any network. Containers do not access the external network or communicate with other containers. You can use it when you want to disable the networking on a container.

#### The Overlay Driver

The Overlay driver is for multi-host network communication, as with Swarm or Docker Kubernetes. It allows containers across the host to communicate with each other without worrying about the setup. Think of an overlay network as a distributed virtualized network that’s built on top of an existing computer network.

To create an overlay network for Docker Swarm services, use the following command:

```bash
$ docker network create -d overlay my-overlay-network
```

To create an overlay network so that standalone containers can communicate with each other, use this command:

```bash
$ docker network create -d overlay --attachable my-attachable-overlay
```

#### The Macvlan Driver

This driver connects Docker containers directly to the physical host network. As per the Docker documentation:

> “Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host’s network stack.”

Macvlan networks are best for legacy applications that need to be modernized by containerizing them and running them on the cloud because they need to be attached to a physical network for performance reasons. A macvlan network is also not supported on Docker Desktop for macOS.

### Basic Docker Networking Commands

To see which commands list, create, connect, disconnect, inspect, or remove a Docker network, use the `docker network help` command.

```bash
$ docker network help
Usage:  docker network COMMAND
Manage networks
Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks
```

Let’s run through some examples of each command, starting with `docker network connect`.

#### Connecting a Container to a Network

Let’s try to connect a container to the `mynetwork` we have created. First, we would need a running container that can connect to `mynetwork`.

```bash
$ docker run -it ubuntu bash
root@0f8d7a833f42:/#
```

Now we have an Ubuntu Linux image and started a login shell as root inside it. We have the container running in interactive mode with the help of the `-it` flags.

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS
0f8d7a833f42   ubuntu    "bash"    9 seconds ago   Up 7 seconds
```

Run the `docker network connect 0f8d7a833f42` command to connect the container named `wizardly_greider` with `mynetwork`. To verify that this container is connected to `mynetwork`, use the `docker inspect` command.

```json
"mynetwork": {
    "IPAMConfig": {},
    "Links": null,
    "Aliases": [
        "0f8d7a833f42"
    ],
    "NetworkID": "97a158252c995d3632560852c62bd140984769c8714b1f990c8133a5c8ae65d3",
    "EndpointID": "db21f395ca781523c115706b11063ebe879cf5ef246c24fd128fe621a582cade",
    "Gateway": "172.20.0.1",
    "IPAddress": "172.20.0.2",
    "IPPrefixLen": 16,
    "IPv6Gateway": "",
    "GlobalIPv6Address": "",
    "GlobalIPv6PrefixLen": 0,
    "MacAddress": "02:42:ac:14:00:02",
    "DriverOpts": {}
}
```

#### Creating a Network

You can use `docker network create mynetwork` to create a Docker network. Here, we’ve created a network named `mynetwork`. Let’s run `docker network ls` to verify that the network is created successfully.

```bash
$ docker network ls
NETWORK ID     NAME        DRIVER    SCOPE
b995772ac197   bridge      bridge    local
7e25f334b07f   host        host      local
97a158252c99   mynetwork   bridge    local
475e50be0fe0   none        null      local
```

Now we have a new custom network named `mynetwork`, and its type is bridge.

#### Disconnecting a Container from the Network

This command disconnects a Docker container from the custom `mynetwork`:

```bash
$ docker network disconnect mynetwork 0f8d7a833f42
```

#### Inspecting the Network

The `docker network inspect` command displays detailed information on one or more networks.

```bash
$ docker network inspect mynetwork
[
    {
        "Name": "mynetwork",
        "Id": "97a158252c995d3632560852c62bd140984769c8714b1f990c8133a5",
        "Created": "2021-03-02T17:36:30.090173896Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.20.0.0/16",
                    "Gateway": "172.20.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0f8d7a833f4283202e905e621e6fd5b29a8e3d4eccc6be6ea0f209f5cb": {
                "Name": "wizardly_greider",
                "EndpointID": "db21f395ca781523c115706b11063ebe879cf5ef",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

#### List Available Networks

Docker installation comes with three networks: none, bridge, and host. You can verify this by running the command `docker network ls`:

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
b995772ac197   bridge    bridge    local
7e25f334b07f   host      host      local
475e50be0fe0   none      null      local
```

#### Removing a Network

The following are the Docker commands to remove a specific or all available networks:

```bash
$ docker network rm mynetwork
mynetwork

$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
b995772ac197   bridge    bridge    local
7e25f334b07f   host      host      local
475e50be0fe0   none        null      local

$ docker network prune
WARNING! This will remove all custom networks not used by at least one container.
Are you sure you want to continue? [y/N]
```

### Public Networking

Let’s talk about how to publish a container port and IP addresses to the outside world. When you start a container using the `docker run` command, none of its ports are exposed. Your Docker container can connect to the outside world, but the outside world cannot connect to the container. To make the ports accessible for external use or with other containers not on the same network, you will have to use the `-P` (publish all available ports) or `-p` (publish specific ports) flag.

For example, here we’ve mapped the TCP port 80 of the container to port 8080 on the Docker host:

```bash
$ docker run -it --rm nginx -p 8080:80
```

Here, we’ve mapped container TCP port 80 to port 8080 on the Docker host for connections to host IP `192.168.1.100`:

```bash
$ docker run -p 192.168.1.100:8085:80 nginx
```

You can verify this by running the following curl command:

```bash
$ curl 192.168.1.100:8085
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and working. Further configuration is required.</p>
<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Let me briefly mention DNS configuration for containers. Docker provides your containers with the ability to make basic name resolutions:

```bash
$ docker exec busybox2 ping www.google.com
PING www.google.com (216.58.216.196): 56 data bytes
64 bytes from 216.58.216.196: seq=0 ttl=37 time=9.672 ms
64 bytes from 216.58.216.196: seq=1 ttl=37 time=6.110 ms

$ ping www.google.com
PING www.google.com (216.58.216.196): 56 data bytes
64 bytes from 216.58.216.196: icmp_seq=0 ttl=118 time=4.722 ms
```

Docker containers inherit DNS settings from the host when using a bridge network, so the container will resolve DNS names just like the host by default. To add custom host records to your container, you’ll need to use the relevant `--dns*` flags outlined in the Docker documentation.

### Docker Compose Networking

Docker Compose is a tool for running multi-container applications on Docker, which are defined using the compose YAML file. You can start your applications with a single command: `docker-compose up`.

By default, Docker Compose creates a single network for each container defined in the compose file. All the containers defined in the compose file connect and communicate through the default network.

If you’re not sure about the commands supported with Docker Compose, you can run the following:

```bash
$ docker compose help
Docker Compose
Usage:
  docker compose [command]
Available Commands:
  build       Build or rebuildাদ
rebuild services
  convert     Converts the compose file to a cloud format (default: cloudformation)
  down        Stop and remove containers, networks
  logs        View output from containers
  ls          List running compose projects
  ps          List containers
  pull        Pull service images
  push        Push service images
  run         Run a one-off command on a service.
  up          Create and start containers
Flags:
  -h, --help   help for compose
Global Flags:
      --config DIRECTORY   Location of the client config files DIRECTORY
  -c, --context string     context
  -D, --debug              Enable debug output in the logs
  -H, --host string        Daemon socket(s) to connect to
Use "docker compose [command] --help" for more information about a command
```

Let’s understand this with an example. In the following `docker-compose.yaml` file, we have a WordPress and a MySQL image.

When deploying this setup, `docker-compose` maps the WordPress container port 80 to port 80 of the host as specified in the compose file. We haven’t defined any custom network, so it should create one for you. Run `docker-compose up -d` to bring up the services defined in the YAML file:

```yaml
version: '3.7'
services:
  db:
    image: mysql:8.0.19
    command: '--default-authentication-plugin=mysql_native_password'
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data:
```

As you can see in the following output, a network named `downloads_default` is created for you:

```bash
$ docker-compose up -d
Creating network "downloads_default" with the default driver
Creating volume "downloads_db_data" with default driver
Pulling db (mysql:8.0.19)...
```

Verify that we have two containers up and running with the `docker ps` command:

```bash
$ docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED
f68265cd6219   wordpress:latest   "docker-entrypoint.s…"   6 minutes ago
2838f5586c73   mysql:8.0.19       "docker-entrypoint.s…"   6 minutes ago
```

Navigate to `http://localhost:80` in your web browser to access WordPress.

Now let’s inspect this network with the `docker network inspect` command. The following is the output:

```bash
$ docker network inspect downloads_default
[
    {
        "Name": "downloads_default",
        "Id": "717ea814aae357ceca3972342a64335a0c910455abf160ed820018b8",
        "Created": "2021-03-05T03:43:42.541707419Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "2838f5586c735894051498c8ed0e5e103209cd22ee5718cbb29e8c6d16": {
                "Name": "downloads_db_1",
                "EndpointID": "10033e064387892253d69ac5813be6bc820a95df",
                "MacAddress": "02:42:ac:

12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "f68265cd6219fb60491c7ebbdae2d7f4c5ea4a74aec9987f5a5082c13b": {
                "Name": "downloads_wordpress_1",
                "EndpointID": "a4a673479ab3d812713955d461cc4d7032aba380",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "downloads",
            "com.docker.compose.version": "1.27.4"
        }
    }
]
```

In the container sections, you can see that two containers (`downloads_db_1` and `downloads_wordpress_1`) are attached to the default `downloads_default` network driver, which is the bridge type. Run the following commands to clean up everything:

```bash
$ docker-compose down
Stopping downloads_wordpress_1 ... done
Stopping downloads_db_1        ... done
Removing downloads_wordpress_1 ... done
Removing downloads_db_1        ... done
Removing network downloads_default
```

You can observe that the network created by Compose is deleted, too:

```bash
$ docker-compose down -v
Removing network downloads_default
WARNING: Network downloads_default not found.
Removing volume downloads_db_data
```

The volume created earlier is deleted, and since the network is already deleted after running the previous command, it shows a warning that the default network is not found. That’s fine.

The example we’ve looked at so far covers the default network created by Compose, but what if we want to create our custom network and connect services to it? You will define the user-defined networks using the Compose file. The following is the `docker-compose.yaml` file:

```yaml
version: '3.7'
services:
  db:
    image: mysql:8.0.19
    command: '--default-authentication-plugin=mysql_native_password'
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    networks:
      - mynetwork
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    networks:
      - mynetwork
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data:
networks:
  mynetwork:
```

I’ve defined a user-defined network under the top-level `networks` section at the end of the file and called the network `mynetwork`. It’s a bridge type, meaning it’s a network on the host machine separated from the rest of the host network stack. Following each service, I added the `network` key to specify that these services should connect to `mynetwork`.

Let’s bring up the services again after changing the Docker Compose YAML file:

```bash
$ docker-compose up -d
Creating network "downloads_mynetwork" with the default driver
Creating volume "downloads_db_data" with default driver
Creating downloads_wordpress_1 ... done
Creating downloads_db_1        ... done
```

As you can see, Docker Compose has created the new custom `mynetwork`, started the containers, and connected them to the custom network. You can inspect it by using the Docker inspect command:

```bash
$ docker network inspect downloads_mynetwork
[
    {
        "Name": "downloads_mynetwork",
        "Id": "cb24ed383ਮ
3832dfdd34ca6fdcf0065c8e0df6b9b72f7b29a2aaada749",
        "Created": "2021-03-05T04:23:14.354570267Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "334bd5bf1689b067fa24705af0b6ab444976b288d25846349ce5b8f691": {
                "Name": "downloads_wordpress_1",
                "EndpointID": "10674d2ddd9feb67c98e3c08fcf451f32bda58e9",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            },
            "9ffc94adab6896620648a1d08d215ba3d9423fee934b905b7bc2a44dd3": {
                "Name": "downloads_db_1",
                "EndpointID": "927943a0202bfb69692bc172a5c26e05676df696",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "mynetwork",
            "com.docker.compose.project": "downloads",
            "com.docker.compose.version": "1.27.4"
        }
    }
]
```

Inspecting the network, you can see there are now two containers connected to the custom network.

---

## Docker Compose Guide

*Created: July 12, 2025*  
*Author: [Saddam Hussain]*

Docker Compose is a powerful tool for defining and running multi-container Docker applications. It simplifies the process of managing complex applications by allowing you to configure services, networks, and volumes in a single YAML file. This guide provides a comprehensive overview of Docker Compose, including its definition, installation, sample configurations, commands, and advanced usage like integrating Dockerfiles and environment variables.

### What is Docker Compose?

Docker Compose is a tool designed to define and manage multi-container Docker applications. It allows developers to specify the services, networks, and volumes required for an application in a single YAML file, typically named `docker-compose.yml`. With Docker Compose, you can start, stop, and scale services with simple commands, making it ideal for development, testing, and production environments.

Key features of Docker Compose include:
- **Configuration in YAML**: Define application services, networks, and volumes in a `docker-compose.yml` file.
- **Single Command Operations**: Start or stop all services with a single command (e.g., `docker-compose up` or `docker-compose down`).
- **Service Scaling**: Scale specific services to handle increased load.
- **Dependency Management**: Specify dependencies between services to ensure proper startup order.
- **Environment Customization**: Use environment variables to make configurations reusable and portable.

The default file name for Docker Compose is `docker-compose.yml`, though other file names can be specified using the `-f` flag.

### Installation of Docker Compose

To use Docker Compose, you must first have Docker installed on your system. Below are the steps to install Docker Compose on a Linux-based system. Note that the version specified in the commands below (1.21.0) is outdated as of July 2025; you should check the [official Docker Compose GitHub releases page](https://github.com/docker/compose/releases) for the latest version.

#### Steps to Install Docker Compose

1. **Install Docker**: Ensure Docker is installed on your system. You can follow the official Docker installation guide for your operating system: [Docker Installation](https://docs.docker.com/engine/install/).

2. **Download Docker Compose**: Use `curl` to download the Docker Compose binary. Replace `1.21.0` with the latest version available.

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

3. **Set Permissions**: Make the binary executable.

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

4. **Create a Symbolic Link**: Create a symbolic link to make `docker-compose` accessible system-wide.

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

5. **Verify Installation**: Check the installed version to confirm successful installation.

```bash
docker-compose --version
```

**Note**: Always use the latest version of Docker Compose for security and feature updates. Replace `1.21.0` with the latest release tag from the Docker Compose GitHub repository.

### Basic Docker Compose Configuration

Below is an example of a `docker-compose.yml` file for a two-tier application consisting of a WordPress service and a MySQL database. This configuration demonstrates key concepts such as services, volumes, ports, and environment variables.

#### Sample `docker-compose.yml`

```yaml
version: '3.7'
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db:3306
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data: {}
```

#### Explanation of the Configuration

- **version**: Specifies the Docker Compose file format version (e.g., `3.7`). The version depends on the Docker Compose version installed.
- **services**: Defines the containers (services) that make up the application.
  - **db**: A MySQL service using the `mysql:5.7` image.
    - **image**: Specifies the Docker image to use.
    - **volumes**: Mounts a named volume (`db_data`) to persist MySQL data.
    - **restart**: Ensures the container restarts automatically if it stops.
    - **environment**: Sets environment variables for MySQL configuration.
  - **wordpress**: A WordPress service using the `wordpress:latest` image.
    - **depends_on**: Ensures the `db` service starts before the `wordpress` service.
    - **ports**: Maps port 8000 on the host to port 80 in the container.
    - **environment**: Configures WordPress to connect to the MySQL database.
- **volumes**: Declares a named volume (`db_data`) for persistent storage.

To run this configuration, navigate to the directory containing the `docker-compose.yml` file and execute:

```bash
docker-compose up -d
```

This command starts the services in detached mode (background). You can access WordPress at `http://localhost:8000`.

### Docker Compose Commands

Docker Compose provides a variety of commands to manage multi-container applications. Below is a list of commonly used commands:

1. **Start Services**:
   ```bash
   docker-compose up
   ```
   Starts all services defined in the `docker-compose.yml` file. Use `-d` for detached mode:
   ```bash
   docker-compose up -d
   ```

2. **Stop and Remove Services**:
   ```bash
   docker-compose down
   ```
   Stops and removes containers, networks, and volumes created by `docker-compose up`.

3. **Restart Services**:
   ```bash
   docker-compose restart
   ```
   Restarts all services.

4. **Stop Services**:
   ```bash
   docker-compose stop
   ```
   Stops running services without removing them.

5. **Start Services**:
   ```bash
   docker-compose start
   ```
   Starts stopped services.

6. **List Containers**:
   ```bash
   docker-compose ps
   ```
   Lists containers created by the Compose file.

7. **Pause Services**:
   ```bash
   docker-compose pause
   ```
   Pauses running services.

8. **Unpause Services**:
   ```bash
   docker-compose unpause
   ```
   Resumes paused services.

9. **View Performance**:
   ```bash
   docker-compose top
   ```
   Displays running processes for each service.

10. **Scale Services**:
    ```bash
    docker-compose up -d --scale db=5
    ```
    Scales the `db` service to run five instances. Note: Services using ports cannot be scaled due to port conflicts.

11. **Use a Custom Compose File**:
    ```bash
    docker-compose -f docker-compose2.yml up -d
    ```
    Runs a specific Compose file (e.g., `docker-compose2.yml`).

12. **Create Containers**:
    ```bash
    docker-compose create
    ```
    Creates containers with the default network but does not start them.

13. **List Images**:
    ```bash
    docker-compose images
    ```
    Lists images used by the services in the Compose file.

14. **Kill Containers**:
    ```bash
    docker-compose kill
    ```
    Forces containers to stop.

15. **View Logs**:
    ```bash
    docker-compose logs
    ```
    Displays logs for all services.

16. **Check Port Mapping**:
    ```bash
    docker-compose port webapp 80
    ```
    Checks if port 80 is bound to the `webapp` service.

17. **Execute Commands in a Container**:
    ```bash
    docker-compose exec webapp ls
    ```
    Runs the `ls` command inside the `webapp` container.

### Running a Dockerfile in Docker Compose

You can use Docker Compose to build and run a custom image defined in a `Dockerfile`. Below is an example of a `docker-compose.yml` file that references a `Dockerfile` in the current directory.

#### Sample `docker-compose.yml` with Dockerfile

```yaml
version: '3'
services:
  webapp:
    build: .
    ports:
      - "5000:5000"
    image: sabair/devops
```

#### Explanation

- **build**: Specifies the directory containing the `Dockerfile` (`.` refers to the current directory).
- **ports**: Maps port 5000 on the host to port 5000 in the container.
- **image**: Names the built image `sabair/devops`.

When you run `docker-compose up`, Docker Compose builds the image from the `Dockerfile` and starts the container.

#### Example `Dockerfile`

```dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

This `Dockerfile` creates a Python-based web application, installs dependencies, and runs `app.py` on port 5000.

### Making Docker Compose More Generic with Environment Variables

To make Docker Compose configurations reusable and environment-agnostic, you can use environment variables. These can be defined in a `.env` file or an external file referenced in the `docker-compose.yml`.

#### Step 1: Create a `.env` File

Create a file named `.env` (or any name, e.g., `env.txt`) with the following content:

```env
NAME=Sabair
ADD=Telangana
```

#### Step 2: Update `docker-compose.yml`

Modify the `docker-compose.yml` to reference the environment file:

```yaml
version: '3'
services:
  webapp:
    build: .
    ports:
      - "5000:5000"
    env_file:
      - env.txt
    image: sabair/devops
```

#### Explanation

- **env_file**: Specifies the file (`env.txt`) containing environment variables.
- The variables `NAME` and `ADD` are available inside the `webapp` container.

#### Alternative: Using `.env` File Directly

Docker Compose automatically loads variables from a `.env` file in the project directory if present. For example:

```env
WORDPRESS_DB_PASSWORD=secretpassword
```

In the `docker-compose.yml`:

```yaml
version: '3'
services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8000:80"
    environment:
      - WORDPRESS_DB_PASSWORD=${WORDPRESS_DB_PASSWORD}
```

When you run `docker-compose up`, the `WORDPRESS_DB_PASSWORD` variable is substituted from the `.env` file.

### Advanced Docker Compose Configuration

#### Custom Networks

To isolate services or improve security, you can define custom networks in the `docker-compose.yml` file. Below is an example that modifies the earlier WordPress and MySQL configuration to use a custom network.

```yaml
version: '3.7'
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    networks:
      - mynetwork
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    networks:
      - mynetwork
    environment:
      - WORDPRESS_DB_HOST=db:3306
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data:
networks:
  mynetwork:
```

#### Explanation

- **networks**: Defines a custom network named `mynetwork` (bridge type by default).
- Each service specifies `mynetwork` under the `networks` key to connect to it.
- Run `docker-compose up -d` to create the network and start the services.

You can inspect the network with:

```bash
docker network inspect downloads_mynetwork
```

This setup ensures that the WordPress and MySQL services communicate over the isolated `mynetwork`.

### Best Practices for Docker Compose

1. **Use Versioned Images**: Specify image versions (e.g., `mysql:5.7` instead of `mysql:latest`) to ensure consistency.
2. **Leverage Named Volumes**: Use named volumes (e.g., `db_data`) for persistent data instead of bind mounts for portability.
3. **Define Dependencies**: Use `depends_on` to control the startup order of services.
4. **Use Environment Files**: Store sensitive or environment-specific variables in `.env` files to keep `docker-compose.yml` generic.
5. **Clean Up Resources**: Run `docker-compose down -v` to remove containers, networks, and volumes after testing.
6. **Avoid Port Conflicts**: When scaling services, ensure they don’t rely on fixed ports to avoid conflicts.
7. **Validate YAML**: Use `docker-compose config` to validate the `docker-compose.yml` file before running.

### Troubleshooting Common Issues

- **Port Conflicts**: If a port is already in use, change the host port (e.g., `8001:80` instead of `8000:80`).
- **Service Not Starting**: Check logs with `docker-compose logs` to diagnose issues like missing environment variables or network errors.
- **Outdated Compose Version**: Ensure the `version` in `docker-compose.yml` matches your Docker Compose installation.
- **Permission Errors**: Verify that the Docker Compose binary has executable permissions (`chmod +x`).

### Conclusion

Docker Compose simplifies the management of multi-container applications by providing a declarative way to define services, networks, and volumes. With a single `docker-compose.yml` file, you can orchestrate complex applications, scale services, and integrate custom Dockerfiles. By using environment variables and custom networks, you can make your configurations portable and secure. This guide covers the essentials of Docker Compose, from installation to advanced usage, empowering you to build and manage Docker-based applications efficiently.

For further reading, refer to the [official Docker Compose documentation](https://docs.docker.com/compose/).

---

## Official Docker documentation URLs:

1. https://docs.docker.com/engine/reference/builder/
2. https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/
3. https://docs.docker.com/engine/reference/builder/#from
4. https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#from
5. https://docs.docker.com/engine/reference/builder/#maintainer
6. https://docs.docker.com/engine/reference/builder/#run
7. https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#run
