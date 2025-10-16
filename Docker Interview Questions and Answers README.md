## 🐳 Docker and Containerization Essentials

### 1\. Syntax of Dockerfile

A Dockerfile is a text file containing **instructions** that Docker reads to **build a Docker image**. Each instruction is a command followed by arguments, executed in order. The general syntax is: **`INSTRUCTION arguments`**.

### 2\. Difference between `CMD`, `ENTRYPOINT`, and `RUN`

| Instruction | Purpose | Execution Time | Usage |
| :--- | :--- | :--- | :--- |
| **`RUN`** | Executes commands and **commits the result to the image** (creates a new layer). | **Image build time** | Installing packages, compiling code. |
| **`CMD`** | Specifies the **default command** to be executed when a container starts. Can be overridden. | **Container run time** | Defining the main application process (e.g., `java -jar app.jar`). |
| **`ENTRYPOINT`** | Specifies the **executable** that will always run when the container starts. Not easily overridden. | **Container run time** | Setting the container's main role (e.g., making the container behave like a specific command). |

### 3\. What is Docker Stack

A Docker Stack is a **group of interrelated services** that share dependencies and can be deployed together to a Docker Swarm cluster using a single YAML file (often a Docker Compose file). It represents a multi-service application.

### 4\. Command to remove unwanted network

The command is **`docker network rm [NETWORK_ID_OR_NAME]`**. To remove all unused networks, use **`docker network prune`**.

### 5\. Can you overwrite environment variables

Yes. Environment variables defined in the Dockerfile (via `ENV`) or Docker Compose file can be **overwritten at runtime** using the **`-e`** flag with `docker run` or by specifying them directly in the Compose file.

### 6\. Difference between Docker and Kubernetes

| Feature | Docker | Kubernetes |
| :--- | :--- | :--- |
| **Role** | **Containerization** (Packaging single application) | **Orchestration** (Managing many containers/nodes) |
| **Scale** | Single node, simple multi-container apps (Compose) | Large-scale clusters, complex microservices |
| **Abstraction** | Image and Container | Pod, Service, Deployment |
| **Self-Healing** | Limited | Robust **self-healing, scaling, and rolling updates** |

### 7\. Command to run Docker Compose file

The command to build and start services defined in a `docker-compose.yml` file is **`docker compose up`** (modern) or **`docker-compose up`** (legacy). Use **`-d`** for detached mode.

### 8\. Components of Docker

The main components of the Docker architecture are:

1.  **Docker Daemon (Server):** The background service that manages images, containers, networks, and volumes.
2.  **Docker Client:** The command-line tool (`docker`) that communicates with the Daemon.
3.  **Docker Registry:** The centralized storage for Docker images (e.g., Docker Hub, AWS ECR).

### 9\. Difference between Docker Image and Container

A **Docker Image** is a **read-only template** containing the application code, dependencies, and configuration needed to run an application. A **Docker Container** is a **runnable instance** of an image, complete with an isolated environment and runtime state.

### 10\. Flow of building a Docker image

1.  Docker client sends the Dockerfile and build context to the Docker Daemon.
2.  The Daemon executes each **`RUN`** instruction in the Dockerfile sequentially.
3.  Each instruction creates a new **read-only layer** on top of the previous layer.
4.  Once all instructions are executed, the final writable layer is flattened to create the completed, immutable image.

### 11\. Difference between Repository and Registry

A **Docker Registry** is a **service** (like Docker Hub or Artifactory) that stores and distributes Docker images. A **Docker Repository** is a **collection of related Docker images** with different tags (versions) within a Registry.

### 12\. Commands to build a Docker image and run a container

  * **Build Image:** **`docker build -t image_name:tag .`**
  * **Run Container:** **`docker run -d -p 8080:80 image_name:tag`**

### 13\. How to optimize Dockerfile

Optimization is based on **layer caching** and **minimizing image size**:

1.  Use **Multi-stage Builds** (Q46).
2.  Combine multiple `RUN` commands into a single instruction using `&& \`.
3.  Place frequently changing instructions (like application code) near the end.
4.  Remove package caches and temporary files immediately after installing packages.

### 14\. Best practices to write Dockerfile

1.  Use **Multi-stage Builds** to separate build-time dependencies from runtime environment.
2.  Use a specific, small **base image** (e.g., Alpine or Distroless).
3.  Use **`.dockerignore`** to exclude unnecessary files from the build context.
4.  Use the **`COPY`** instruction instead of **`ADD`** unless you need TAR extraction.
5.  Set a non-root **`USER`** for security.

### 15\. Can Docker containers communicate with each other

Yes, they communicate via **Docker networks**. Containers on the same user-defined bridge network can resolve each other using their container names as hostnames.

### 16\. Command to get running containers

The command is **`docker ps`**. Use **`docker ps -a`** to list all containers (including stopped ones).

### 17\. How to remove unused containers and dangling images

  * **Remove unused containers:** **`docker container prune`**
  * **Remove dangling images:** **`docker image prune`** or the comprehensive command **`docker system prune`** (removes containers, networks, images, and build cache).

### 18\. What are dangling images

Dangling images are **untagged, intermediary layers** that are no longer referenced by any named image. They are the result of building a new image that replaces an existing one without removing the old, intermediate layers.

### 19\. Moving an image to another server without using a registry concept

Use the **`docker save`** and **`docker load`** commands:

1.  **Server 1 (Source):** **`docker save -o myapp.tar myapp:latest`** (Saves image to a TAR file).
2.  **Transfer:** Move `myapp.tar` to the destination server (e.g., via `scp`).
3.  **Server 2 (Destination):** **`docker load -i myapp.tar`** (Loads image from the TAR file).

### 20\. Difference between Docker Stop and Docker Kill

  * **`docker stop`:** Sends a **SIGTERM** signal to the container's main process, allowing it a grace period (default 10 seconds) to shut down gracefully before sending a SIGKILL.
  * **`docker kill`:** Immediately sends a **SIGKILL** signal to the container, forcing it to terminate immediately without any cleanup.

### 21\. Difference between Docker Run and Docker Create

  * **`docker create`:** **Creates a container** from an image but **does not start it**.
  * **`docker run`:** **Creates a container** from an image and **starts it immediately**. It is equivalent to running `docker create` followed by `docker start`.

### 22\. How to mention environment variables

1.  **Dockerfile:** Using the **`ENV`** instruction.
2.  **`docker run`:** Using the **`-e KEY=VALUE`** flag.
3.  **Docker Compose:** Using the **`environment:`** key under the service definition.

### 23\. How to use secrets in Docker

Secrets are used to manage sensitive data (passwords, tokens).

1.  **Docker Swarm:** Use **`docker secret create`** and mount the secret into the service definition.
2.  **Docker Compose (Standalone):** Use **Docker Secrets** defined under the top-level `secrets` key and referenced under the service's `secrets` key.

### 24\. How to troubleshoot issues in any container

1.  **Check Logs:** **`docker logs container_name`**.
2.  **Inspect State:** **`docker inspect container_name`** (check status, volumes, network).
3.  **Attach/Execute:** **`docker attach container_name`** or **`docker exec -it container_name /bin/bash`** to run commands interactively inside the container.
4.  **Resource Check:** Check the host's CPU/memory usage.

### 25\. Difference between `COPY` and `ADD`

  * **`COPY`:** Copies files/directories from the local path to the container filesystem. It is the preferred, transparent method.
  * **`ADD`:** Has all the functionality of `COPY` but with **extra features**: it can extract compressed TAR archives and can fetch files from a remote URL. Due to its implicit behavior, it's generally discouraged.

### 26\. How to store data in Docker containers

Data persistence is achieved primarily through **Volumes** and **Bind Mounts**:

  * **Volumes:** The preferred method. Docker manages the data on the host machine, making it easy to back up or move.
  * **Bind Mounts:** Directly maps a file or directory from the host machine to a path in the container.

### 27\. Explain networking in Docker

Docker uses the **Container Network Model (CNM)** to manage networks. Key network drivers include:

  * **Bridge (Default):** Creates a private internal network for containers on a single host. Containers can communicate using IP addresses or service names.
  * **Host:** Removes network isolation; containers share the host's network stack.
  * **Overlay:** Used in **Docker Swarm** for multi-host container communication.

### 28\. Difference between Dockerfile and Docker Compose file

  * **Dockerfile:** Defines the **contents and environment** of a **single container image** (the "how-to-build" recipe).
  * **Docker Compose file (YAML):** Defines **multi-container applications** (services, networks, volumes) and their inter-dependencies for deployment on a single host (the "how-to-run-together" recipe).

### 29\. If one service is dependent on another, how to create it

In a Docker Compose file, use the **`depends_on`** key under the dependent service definition to specify start-up order. For example, a web app depends on the database:

```yaml
services:
  database:
    # ... db config
  web:
    depends_on:
      - database # web service waits for the database service to start
```

-----

## 🐋 Docker Swarm and Orchestration

### 30\. What is Docker Stack

See Q3. A Docker Stack is an **application deployment model** representing multiple services defined in a Compose file, deployed and managed by a **Docker Swarm** cluster.

### 31\. Difference between Docker Swarm and Kubernetes

Docker Swarm is Docker's **native, simpler, and lighter** orchestration tool, often used for smaller deployments and quick setup. **Kubernetes** is the **industry standard, robust, and highly complex** orchestrator used for massive, multi-cloud deployments. Swarm is easier to learn; Kubernetes is more powerful.

### 32\. How to do OS upgrades in containers

OS upgrades are done by **rebuilding the image** with a new, updated **base image** tag in the Dockerfile (e.g., changing `FROM ubuntu:20.04` to `FROM ubuntu:22.04`). You do not upgrade the OS inside a running container.

### 33\. Creating service on a particular node

In Docker Swarm, services are deployed to specific nodes using **constraints** (a form of node labeling) in the `docker service create` command:
**`docker service create --constraint 'node.labels.role==db-node' [IMAGE]`**

### 34\. What is Docker Swarm

Docker Swarm is a **native clustering and orchestration solution** for Docker. It allows you to turn a pool of Docker hosts into a secure, highly available cluster.

### 35\. Architecture of Docker Swarm

Swarm uses a **Manager-Worker** architecture:

  * **Manager Nodes:** Handle the cluster state, scheduling, and service management. They use the Raft consensus algorithm for synchronization.
  * **Worker Nodes:** Execute the tasks (containers) deployed by the manager.

### 36\. How to join worker nodes to master

The manager node generates a **join token** using **`docker swarm join-token worker`**. This token is then executed on the worker node to securely join it to the Swarm cluster.

### 37\. Command to create service

In a Docker Swarm cluster, services are created using **`docker service create [OPTIONS] IMAGE [COMMAND]`**.

### 38\. Sample Docker Compose file

```yaml
version: '3.8' # Specify Compose file format version
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - app-net
  api:
    build: .
    environment:
      API_HOST: database
    networks:
      - app-net
networks:
  app-net:
```

### 39\. Two modes of services created

In Docker Swarm, services can be created in two modes:

1.  **Replicated:** Runs a **specified number of identical tasks** (containers) across the cluster (e.g., 3 replicas of a web server).
2.  **Global:** Runs **exactly one task** (container) on **every available node** in the swarm (e.g., a logging or monitoring agent).

### 40\. Number of clusters generally used in your company

*(Tailor to your experience)* We typically use **one production Kubernetes cluster** and **one staging/QA Kubernetes cluster** for application deployment. Development environments use local Docker Compose setups.

### 41\. Number of containers in your current project

*(Tailor to your experience)* Our microservices architecture involves approximately **15 to 20 containers** running simultaneously in production, representing various services, a shared database, caching, and monitoring agents.

### 42\. Version of Docker being used

*(Tailor to your experience)* We use the latest stable **Docker Engine (e.g., 24.x)** and the corresponding **Docker Compose CLI (V2)**.

### 43\. Multi-host networking with Docker Swarm

Docker Swarm uses the **Overlay Network Driver**. The manager node distributes the overlay network configuration, allowing containers running on different physical host machines to communicate securely as if they were on the same network.

### 44\. How to pass arguments in Dockerfile

Arguments are passed using the **`ARG`** instruction in the Dockerfile. They are defined at **build time** and can be passed via the `docker build` command using the **`--build-arg KEY=VALUE`** flag.

### 45\. Command to give a custom Dockerfile name while building an image

Use the **`-f`** flag: **`docker build -t myapp:latest -f CustomDockerfile .`**

### 46\. What is multi-stage Docker build?

Multi-stage builds involve using **multiple `FROM` statements** in a single Dockerfile. The concept is to use the first stage to **build the application** (with large toolchains) and the final stage to **copy only the necessary runtime artifacts** (the compiled binary/JAR) into a much smaller base image, minimizing the final image size.

### 47\. What are the stages in a Docker image build?

Stages are defined by consecutive **`FROM`** instructions in a Dockerfile. In a multi-stage build, common stages are:

1.  **`builder` stage:** Contains the SDK and tools to compile code.
2.  **`final` stage:** Contains only the JRE/runtime environment needed to execute the compiled artifact.

### 48\. Why do we use `ENTRYPOINT` and `CMD` instructions?

We often use them together for a specific pattern: **`ENTRYPOINT`** sets the **fixed executable**, and **`CMD`** sets the **default parameters** for that executable. This makes the container behave like a standard command-line tool.

### 49\. How do you scan Docker images—both during build and at the registry level? Are you using any extensions or tools for image scanning?

  * **During Build:** We use **Trivy** in the CI pipeline to scan the image immediately after creation, using the command **`trivy fs .`** to scan the filesystem before pushing.
  * **At Registry Level:** We integrate our registry (**Artifactory/AWS ECR**) with tools like **JFrog Xray** or **Clair** to automatically scan images upon push, enforcing a continuous security policy.

### 50\. How do you pass environment variables during Docker build commands?

Environment variables are passed at build time using the **`--build-arg`** flag, which corresponds to the **`ARG`** instruction in the Dockerfile.

### 51\. What services do you use for storing Docker images?

*(Tailor to your experience)* We use **AWS Elastic Container Registry (ECR)** (or **Azure Container Registry/Google Container Registry**), which is integrated with our cloud platform, and **JFrog Artifactory** as a centralized, private registry.

### 52\. Which container registry do you use for storing Docker images?

*(Tailor to your experience)* We use **JFrog Artifactory** (or **Nexus Repository Manager**) as our main private registry.
