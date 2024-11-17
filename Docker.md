# 55 Common Docker Interview Questions

## 1. What is _Docker_, and how is it different from _virtual machines_?

**Docker** is a **containerization** platform that simplifies application deployment by ensuring software and its dependencies run uniformly on any infrastructure, from laptops to servers to the cloud.

Using Docker allows you to**bundle code and dependencies** into a container image you can then run on any Docker-compatible environment. This approach is a significant improvement over traditional virtual machines, which are less efficient and come with higher overheads.

### Key Docker Components

- **Docker Daemon**: A persistent background process that manages and executes containers.
- **Docker Engine**: The CLI and API for interacting with the daemon.
- **Docker Registry**: A repository for Docker images.

### Core Building Blocks

- **Dockerfile**: A text document containing commands that assemble a container image.
- **Image**: A standalone, executable package containing everything required to run a piece of software.
- **Container**: A runtime instance of an image.

### Virtual Machines vs. Docker Containers

#### Virtual Machines

- **Advantages**:

  - Isolation: VMs run separate operating systems, providing strict application isolation.

- **Inefficiencies**:
  - Resource Overhead: Each VM requires its operating system, consuming RAM, storage, and CPU. Running multiple VMs can lead to redundant resource use.
  - Slow Boot Times: Booting a VM involves starting an entire OS, slowing down deployment.

#### Containers

- **Efficiencies**:

  - Resource Optimizations: As containers share the host OS kernel, they are exceptionally lightweight, requiring minimal RAM and storage.
  - Rapid Deployment: Containers start almost instantaneously, accelerating both development and production.

- **Isolation Caveats**:
  - Application-Level Isolation: While Docker ensures the separation of containers from the host and other containers, it relies on the host OS for underlying resources.

### Code Example: Dockerfile

Here is the `Dockerfile`:

```Dockerfile
FROM python:3.8

WORKDIR /app

COPY requirements.txt requirements.txt

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

### Core Unique Features of Docker

- **Layered File System**: Docker images are composed of layers, each representing a set of file changes. This structure aids in minimizing image size and optimizing builds.

- **Container Orchestration**: Technologies such as Kubernetes and Docker Swarm enable the management of clusters of containers, providing features like load balancing, scaling, and automated rollouts and rollbacks.

- **Interoperability**: Docker containers are portable, running consistently across diverse environments. Additionally, Docker complements numerous other tools and platforms, including Jenkins for CI/CD pipelines and AWS for cloud services.
  <br>

## 2. Can you explain what a _Docker image_ is?

A **Docker image** is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and configuration files.

It provides consistency across environments by ensuring that each instance of an image is identical, a key principle of **Docker's build-once-run-anywhere** philosophy.

### Image vs. Container

- **Image**: A static package that encompasses everything the application requires to run.
- **Container**: An operating instance of an image, running as a process on the host machine.

### Layered File System

Docker images comprise multiple layers, each representing a distinct file system modification. Layers are read-only, and the final container layer is read/write, which allows for efficiency and flexibility.

### Key Components

- **Operating System**: Traditional images have a full or bespoke OS tailored for the application's needs. Recent developments like "distroless" images, however, focus solely on application dependencies.
- **Application Code**: Your code and files, which are specified during the image build.

### Image Registries

Images are stored in **Docker image registries** like Docker Hub, which provides a central location for image management and sharing. You can download existing images, modify them, and upload the modified versions, allowing teams to collaborate efficiently.

### How to Build an Image

1. **Dockerfile**: Describes the steps and actions required to set up the image, from selecting the base OS to copying the application code.
2. **Build Command**: Docker's build command uses the Dockerfile as a blueprint to create the image.

### Advantages of Docker Images

- **Portability**: Docker images ensure consistent behavior across different environments, from development to production.
- **Reproducibility**: If you're using the same image, you can expect the same application behavior.
- **Efficiency**: The layered filesystem reduces redundancy and accelerates deployment.
- **Security**: Distinct layers permit granular security control.

### Code Example: Dockerfile

Here is the Dockerfile:

```docker
# Use a base image
FROM ubuntu:latest

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Specify the command to run on container start
CMD ["/bin/bash"]
```

### Best Practices for Dockerfiles

- Use the official base image if possible.
- Aim for minimal layers for better efficiency.
- Regularly update the base image to ensure security and feature updates.
- Reduce the number of packages installed to minimize security risks.
  <br>

## 3. How does a _Docker container_ differ from a _Docker image_?

**Docker images** serve as templates for containers, whereas **Docker containers** are running instances of those images.

### Key Distinctions

- **State**: Containers encapsulate both the application code and its runtime environment in a stable and consistent **state**. In contrast, images are passive and don't change once created.

- **Mutable vs Immutable**: Containers, like any running process, can modify their state. In contrast, images are **immutable** and do not change once built.

- **Disk Usage**: Containers have both writable layers (such as logs or configuration files) and read-only layers (the image layers), potentially leading to increased disk usage over time. Docker's use of layered storage, however, limits this growth.

Images, on the other hand, are solely read-only, meaning each instance based on the same image doesn't consume additional disk space.

![Docker Image vs Container](<https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/docker%2Fdocker-image-vs%20docker-container%20(1).png?alt=media&token=7ae72ca9-6342-45f0-93e6-fffc8265570d>)

### Practical Demonstration

Here is the code:

1.  **Dockerfile** - Defines the image:

```docker
# Set the base image
FROM python:3.8

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

2. **Building an Image** - Use the `docker build` command to create the image.

```bash
docker build -t myapp .
```

3. **Instantiating Containers** - Run the built image with `docker run` to spawn a container.

```bash
# Run a single command within a new container
docker run myapp python my_script.py
# Run a container in detached mode and enter it to explore the environment
docker run -d -it --name mycontainer myapp /bin/bash
```

4. **Viewing Containers** - The `docker container ls` or `docker ps` commands display active containers.

5. **Modifying Containers** - As an example, you can change the content of a container by entering in via `docker exec`.

```bash
docker exec -it mycontainer /bin/bash
```

6. **Stopping and Removing Containers** - This can be done using the `docker stop` and `docker rm` commands or combined with the `-f` flag.

```bash
docker stop mycontainer
docker rm mycontainer
```

7. **Cleaning Up Images** - Remove any unused images to save storage space.

```bash
docker image prune -a
```

<br>

## 4. What is the _Docker Hub_, and what is it used for?

The **Docker Hub** is a public cloud-based registry for Docker images. It's a central hub where you can find, manage, and share your Docker container images. Essentially, it is a **version control system** for Docker containers.

### Key Functions

- **Image Storage**: As a centralized repository, the Hub stores your Docker images, making them easily accessible.

- **Versioning**: It maintains a record of different versions of your images, enabling you to revert to previous iterations if necessary.

- **Collaboration**: It's a collaborative platform where multiple developers can work on a project, each contributing to and pulling from the same image.

- **Link to GitHub**: Docker Hub integrates with the popular code-hosting platform GitHub, allowing you to automatically build images using pre-defined build contexts.

- **Automation**: With automated builds, you can rest assured that your images are up-to-date and built to the latest specifications.

- **Webhooks**: These enable you to trigger external actions, like CI/CD pipelines, when certain events occur, enhancing the automation capabilities of your workflow.

- **Security Scanning**: Docker Hub includes security features to safeguard your containerized applications. It can scan your images for vulnerabilities and security concerns.

### Cost and Pricing

- **Free Tier**: Offers one private repository and unlimited public repositories.
- **Pro and Team Tiers**: Both come with advanced features. The Team tier provides collaboration capabilities for organizations.

### Use Cases

- **Public Repositories**: These are ideal for sharing your open-source applications with the community. Docker Hub is home to a multitude of public repositories, each extending the functionality of Docker.

- **Private Repositories**: For situations requiring confidentiality, or to ensure compliance in regulated environments, Docker Hub allows you to maintain private repositories.

### Key Benefits and Limitations

- **Benefits**:

  - Centralized Container Distribution
  - Security Features
  - Integration with CI/CD Tools
  - Multi-Architecture Support

- **Limitations**: - Limited Private Repositories in the Free Plan - Might Require Additional Security Measures for Sensitive Workloads
  <br>

## 5. Explain the _Dockerfile_ and its significance in _Docker_.

One of the defining features of **Docker** is its use of `Dockerfiles` to automate the creation of container images. A **Dockerfile** is a text document that contains all the commands a user could call on the command line to assemble an image.

### Common Commands

- **FROM**: Sets the base image for subsequent build stages.
- **RUN**: Executes commands within the image and then commits the changes.
- **EXPOSE**: Informs Docker that the container listens on a specific port.
- **ENV**: Sets environment variables.
- **ADD/COPY**: Adds files from the build context into the image.
- **CMD/ENTRYPOINT**: Specifies what command to run when the container starts.

### Multi-Stage Builds

- **FROM**: Allows for multiple build stages in a single `Dockerfile`.
- **COPY --from=source**: Enables copying from another build stage, useful for extracting build artifacts.

### Image Caching

Docker uses caching to speed up build processes. If a layer changes, Docker rebuilds it and all those that depend on it. Often, this results in fortuitous cache misses, making builds slower than anticipated.

To optimize, place commands that change frequently (such as file copying or package installation) toward the end of the file.

Docker Build Accesses a remote repository, the Docker Cloud. The build context is the absolute path or URL to the directory containing the `Dockerfile`.

### Tips for Writing Efficient Dockerfiles

- **Use Specific Base Images**: Start from the most lightweight, appropriate image to keep your build lean.
- **Combine Commands**: Chaining commands with `&&` (where viable) reduces layer count, enhancing efficiency.
- **Remove Unneeded Files**: Eliminate files your application doesn't require, especially temporary build files or cached resources.

### Code Example: Dockerfile for a Node.js Web Server

Here is the `Dockerfile`:

```dockerfile
# Use a specific version of Node.js as the base
FROM node:14-alpine

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json first to leverage caching when the
# dependencies haven't changed
COPY package*.json ./

# Install NPM dependencies
RUN npm install --only=production

# Copy the rest of the application files
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the Node.js application
CMD ["node", "app.js"]
```

<br>

## 6. How does _Docker_ use _layers_ to build images?

**Docker** follows a **Layered File System** approach, employing Union File Systems like **AUFS**, **OverlayFS**, and **Device Mapper** to stack image layers.

This structure enhances modularity, storage efficiency, and image-building speed. It also offers read-only layers for image consistency and integrity.

### Union File Systems

**Union File Systems** permit stacking multiple directories or file systems, presenting them **coherently** as a single unit. While several such systems are in use, **AUFS** and **OverlayFS** are notably popular.

1. **AUFS**: A front-runner for a long time, AUFS offers versatile compatibility but is not part of the Linux kernel.
2. **OverlayFS**: Now integrated into the Linux kernel, OverlayFS is lightweight and provides backward compatibility with `ext4` and `XFS`.

### Image Layering in Docker

When stacking Docker image layers, it's akin to a file system with **read-only** layers superimposed by a **writable** layer, the **container layer**. This setup ensures separation and persistence:

1. **Base Image Layer**: This is the foundation, often comprising the operating system and core utilities. It's mostly read-only to safeguard uniformity.

2. **Intermediate Layers**: These are **interchangeable** and encapsulate discrete modifications. Consequently, they are also **mostly read-only**.

3. **Topmost or Container Layer**: This layer records real-time alterations made within the container and is mutable.

### Code Overlayers

Here is the code:

1. Each layer is defined by a `Dockerfile` instruction.
2. The base image is `ubuntu:latest`, and the application code is stored in a file named `app.py`.

```docker
# Layer 1: Start from base image
FROM ubuntu:latest

# Layer 2: Set the working directory
WORKDIR /app

# Layer 3: Copy the application code
COPY app.py /app

# Placeholder for Dockerfile
# ...
```

<br>

## 7. What's the difference between the `COPY` and `ADD` commands in a _Dockerfile_?

Let's look at the subtle distinctions between the `COPY` and `ADD` commands within a Dockerfile.

### Purpose

- **COPY**: Designed for straightforward file and directory copying. It's the preferred choice for most use-cases.
- **ADD**: Offers additional features such as URI support. However, since it's more powerful, it's often **recommended to stick with `COPY`** unless you specifically need the extra capabilities.

### Key Distinctions

- **URI and TAR Extraction**: Only `ADD` allows you to use URIs (including HTTP URLs) as well as automatically extract local .tar resources. For simple file transfers, `COPY` is the appropriate choice.
- **Cache Considerations**: Unlike `COPY`, which respects image build cache, `ADD` bypasses cache for any resources that differ even slightly from their cache entries. This can lead to slower builds.
- **Security Implications**: Since `ADD` permits downloading files at build-time, it introduces a potential security risk point. In scenarios where the URL isn't controlled, and the file isn't carefully validated, prefer `COPY`.
- **File Ownership**: While both `COPY` and `ADD` maintain file ownership and permissions during the build process, there might be OS-specific deviations. Consistent behavior is often a critical consideration, making `COPY` the safer choice.
- **Simplicity and Transparency**: Using `COPY` exclusively, when possible, ensures clarity and simplifies Dockerfile management. For instance, it's easier for another developer or a CI/CD system to comprehend a straightforward `COPY` command than to ascertain the intricate details of an `ADD` command that incorporates URL-based file retrieval or TAR extraction.

### Best Practices

- **Avoid Web-Based Transfers**: Steer clear of resource retrieval from untrusted URLs within Dockerfiles. It's safer to copy these resources into your build context, ensuring security and reproducibility.

- **Cache Management**: Because `ADD` can bypass caching for resources that are even minimally different from their cached versions, it can inadvertently lead to slowed build processes. To avoid this, prefer the deterministic, cache-friendly behavior of `COPY` whenever plausible.
  <br>

## 8. What’s the purpose of the `.dockerignore` file?

The **`.dockerignore`** file, much like `gitignore`, is a list of patterns indicating which files and directories should be **excluded** from image builds.

Using this file, you can optimize the **build context**, which is the set of files and directories sent to the Docker daemon for image creation.

By excluding unnecessary files, such as build or data files, you can reduce the build duration and optimize the size of the final Docker image. This is important for **minimizing container footprint** and enhancing overall Docker efficiency.
<br>

## 9. How would you go about creating a _Docker image_ from an existing _container_?

Let's look at each of the two main methods:

### `docker container commit` Method:

For simple use cases or quick image creation, this method can be ideal.

It uses the following command:

```bash
docker container commit <CONTAINER_ID> <REPOSITORY:TAG>
```

Here's a detailed example:

Say you have a running container derived from the `ubuntu` image and nicknamed 'my-ubuntu'.

1. Start the container:

   ```bash
   docker run --interactive --tty --name my-ubuntu ubuntu
   ```

2. For instance, you decide to customize the `my-ubuntu` container by adding a package.

3. **Make the package change** (for this example):

   ```bash
   docker exec -it my-ubuntu bash  # Enter the shell of your 'my-ubuntu' container
   apt update
   apt install -y neofetch         # Install `neofetch` or another package for illustration
   exit                           # Exit the container's shell
   ```

4. Take note of the **"Container ID"** using `docker ps` command:

   ```bash
   docker ps
   ```

   You will see output resembling:

   ```plaintext
   CONTAINER ID        IMAGE               COMMAND             ...        NAMES
   f2cb54bf4059        ubuntu              "/bin/bash"         ...        my-ubuntu
   ```

   In this output, "f2cb54bf4059" is the Container ID for 'my-ubuntu'.

5. Use the `docker container commit` command to **create a new image** based on changes in the 'my-ubuntu' container:

   ```bash
   docker container commit f2cb54bf4059 my-ubuntu:with-neofetch
   ```

   Now, you have a modified image based on your updated container. You can verify it by running:

   ```bash
   docker run --rm -it my-ubuntu:with-neofetch neofetch
   ```

Here, **"f2cb54bf4059"** is the Container ID that you can find using **`docker ps`**.

### Image Build Process Method:

This method provides more control, especially in intricate scenarios. It generally involves a two-step process where you start by creating a `Dockerfile` and then build the image using **`docker build`**.

#### Steps:

1.  **Create A `Dockerfile`**: Begin by preparing a `Dockerfile` that includes all your customizations and adjustments.

For our 'my-ubuntu' example, the `Dockerfile` can be as simple as:

    ```Dockerfile
    FROM my-ubuntu:latest
    RUN apt update && apt install -y neofetch
    ```

2.  **Build the Image**: Enter the directory where your `Dockerfile` resides and start the build using the following command:

    ```bash
    docker build -t my-ubuntu:with-neofetch .
    ```

Subsequently, you can run a container using this new image and verify your modifications:

```bash
docker run --rm -it my-ubuntu:with-neofetch neofetch
```

<br>

## 10. In practice, how do you reduce the size of _Docker images_?

Reducing **Docker image sizes** is crucial for efficient resource deployment. You can achieve this through various strategies.

### Multi-Stage Builds

**Multi-Stage Builds** allow you to use multiple `Dockerfile` stages, segregating different aspects of your build process. This enables a cleaner separation between build-time and run-time libraries, ultimately leading to smaller images.

Here is the `dockerfile` with the multi-stage build.

```Dockerfile

# Use an official Node.js runtime as the base image
FROM node:current-slim AS build

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json files to the workspace
COPY package*.json ./

# Install app dependencies
RUN npm install

# Copy the entire project into the container
COPY . .

# Build the app
RUN npm run build

# Use a smaller base image for the final stage
FROM node:alpine AS runtime

# Set the working directory in the container
WORKDIR /app

# Copy built files and dependency manifest
COPY --from=build /app/package*.json ./
COPY --from=build /app/dist ./dist

# Install production dependencies
RUN npm install --only=production

# Specify the command to start the app
CMD ["node", "dist/main.js"]
```

The `--from` flag in the `COPY` and `RUN` instructions is key here, as it allows you to select artifacts from a previous build stage.

### .dockerignore File

Similar to `.gitignore`, the `.dockerignore` file excludes files and folders from the Docker build context. This can significantly **reduce the size** of your build context, leading to slimmer images.

Here is an example of a `.dockerignore` file:

```plaintext
node_modules
npm-debug.log
```

### Using Smaller Base Images

Selecting a minimalistic base image can lead to significantly smaller containers. For node.js, you can choose a smaller base image such as `node:alpine`, especially for production use. The `alpine` version is particularly lightweight as it's built on the Alpine Linux distribution.

Here are images with different sizes:

- node:current-slim (about 200MB)
- node:alpine (about 90MB)
- node:current (about 900MB)

### One-Time Execution Commands

Using `RUN` and multi-line `COPY` commands within the same `Dockerfile` layer can lead to image bloat. To mitigate this, leverage a single `RUN` command that packages multiple operations. This approach reduces additional layer creation, resulting in smaller images.

Here is an example:

```Dockerfile
RUN apt-get update && apt-get install -y nginx && apt-get clean
```

Ensure that you always combine such commands in a single `RUN` instruction, separated by logical operators like `&&`, and clean up any temporary files or caches to keep the layer minimal.

### Package Managers and Caching

When using package managers like `npm` and `pip` in your images, it's important to use a **`--production`** flag.

For `npm`, running the following command prevents the installation of development dependencies:

```dockerfile
RUN npm install --only=production
```

For `pip`, you can achieve the same with:

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

This practice significantly reduces the image size by only including necessary runtime dependencies.

### Utilize Glob Patterns for `COPY`

When using the `COPY` command in your `Dockerfile`, it's best to introduce `.dockerignore` syntax to ensure only essential files are copied.

Here is an example:

```Dockerfile
COPY ["*.json", "*.sh", "config/", "./"]
```

<br>

## 11. What command is used to run a _Docker container_ from an _image_?

The lean, transformed and updated version of the answer includes all the essential points.

To **run a Docker container from an image**, you can use the `docker run` command:

### docker run

The command `docker run` combines several actions:

- **Creating**: If the container matching the input name already exists, it will stop and then start again.
- **Running**: Activates the container, starting its process.
- **Linking**: Connects to the necessary network, storage, and system resources.

### Basic Usage

Here is the generic structure:

```bash
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

### Practical Example

```bash
docker run -d -p 5000:5000 --name myapp myimage:latest
```

In this example:

- `-d`: The container is detached, running in the background.
- `-p 5000:5000`: The host port 5000 is mapped to the container port 5000.
- `--name myapp`: The container is named `myapp`.
- `myimage:latest`: The image used is `myimage` with the `latest` tag.

### Additional Options and Example

Here is an alternative command:

```bash
docker run --rm -it -v /host/path:/container/path myimage:1.2.3 /bin/bash
```

This:

- Deletes the container after it stops.
- Opens an interactive terminal.
- Mounts the host's `/host/path` to the container's `/container/path`.
- Uses the command `/bin/bash` when starting the container.
  <br>

## 12. Can you explain what a _Docker namespace_ is and its benefits?

A **Docker namespace** uniquely identifies Docker objects like containers, images, and volumes. Namespaces streamline resource organization and data isolation, supporting your security and operational requirements.

### Advantages of Docker Namespaces

- **Isolated Environment**: Ensures separation, vital for multi-tenant systems, in\-house CI/CD, and staging environments.

- **Resource Segregation**: Every workspace allocates distinct processes, network ports, and filesystem mounts.

- **Multi-Container Management**: You can track related containers across various environments thoroughly.

- **Improved Debugging and Error Control**: Dockers namespace, keep your workstations clean and facilitate accurate error tracking.

- **Enhanced Security**: Reduces the risk of data breaches and system interdependencies.

- **Portability and Adaptability**: Supports a consistent operational model, irrespective of the environment.

### Key Namespace Types

- **Image IDs**: Unique identifiers for Docker images.
- **Container Names**: Provides friendly readability to Docker containers.
- **Volume Names**: Simplified references in managing persistent data volumes.

### Code Example: Working with Docker Namespaces

Here is the Python code:

```python
import docker

# Establish connection with Docker daemon
client = docker.from_env()

# Pull a Docker image
client.images.pull('ubuntu:latest')

# List existing Docker images
images = client.images.list()
print(images)

# Note: In a practical Docker environment, you would see more detailed output related to the images.

# Retrieve a container by its name
event_container = client.containers.get('event-container')

# Inspect a specific container to gather detailed information
inspect_data = event_container.attrs
print(inspect_data)

# Create a new Docker volume
client.volumes.create('my-named-volume')
```

<br>

## 13. What is a _Docker volume_, and when would you use it?

A **Docker volume** is a directory or file within a Docker Host's writable layer that isn't tied to a specific container. This decoupling allows data persistence, even after containers have been stopped or removed.

### Volume Types

1. **Host-Mounted Volumes**: These link a directory on the host machine to the container.
2. **Named Volumes**: They have a specific name and are managed by Docker.
3. **Anonymous Volumes**: These are generated by Docker and not tied to a specific container or its data.

### Use Cases

Docker volumes are fundamental for data storage and sharing, which is especially beneficial in microservice and stateful applications.

- **File Sharing**: Volume remaps between containers, facilitating file sharing without needing to commit volumes to an image or set up additional systems like NFS.
- **Database Management**: Ensures database consistency by isolating **database files** within volumes. This makes it simpler to back up and restore databases.

- **Stateful Container Handling**: Volumes assist in preserving stateful container data, like logs or configuration files, ensuring uninterrupted service data delivery and persistence, even in case of container updates or failures.

- **Configuration and Secret Management**: Volumes provide an excellent way to mount **configuration files** and secrets. This can help you secure sensitive data and reduces the need to build it into the application.

- **Backup and Restore**: By using volumes, you can separate your data from the lifecycle of the container. It becomes easier to back them up and restore them in the event of data loss.
  <br>

## 14. Explain the use and significance of the `docker-compose` tool.

**Docker Compose**, a command-line tool, facilitates multi-container Docker applications, using a YAML file to define their architecture and how they interconnect. This is incredibly useful for setting up multi-container environments and facilitates a "one command" startup for all relevant components. For instance, a **web application** might require a backend database, a message queue, and more. While you can launch these components individually, using `docker-compose` makes it a seamless single-command operation.

### Core Advantages

- **Simplified Multi-Container Management**: With one predefined configuration, launch and manage multi-container apps effortlessly.
- **Streamlined Environment Sharing**: Consistent setups between teams and environments simplify testing, staging, and development.
- **Automatic Inter-Container Networking**: Defines network configurations such as volume sharing and service linking without added commands.
- **Parallel Service Startup**: Efficiently starts services in parallel, making boot-ups faster.

### Core Components

- **Services**: Containers that build off the same image, defined in the compose file. Each is an independent component (e.g., web server, database).
- **Volumes**: For persistent data, decoupled from container lifespan. Useful for databases, among others.
- **Networks**: Virtual networks for isolating different applications or services, keeping them separate or aiding in communication.

### YAML Configuration Example

Here is the YAML configuration:

```yaml
version: "3.3"

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - "/path/to/html:/usr/share/nginx/html"
    depends_on:
      - db

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: dbname
    volumes:
      - /my/own/datadir:/var/lib/postgresql/data

networks:
  backend:
    driver: bridge
```

- **Services**: `web` and `db` are the components mentioned. They define an image to be used, port settings, volumes for data persistence, and dependency structures (like how `web` depends on `db`).
- **Volumes**: The `db` service has a volume specified for persistent storage.
- **Networks**: The `web` and `db` services are part of the `backend` network, defined at the bottom. This assures consistent networking, even when services get linked or containers restarted.
  <br>

## 15. Can _Docker containers_ running on the same _host_ communicate with each other by default? If so, how?

Yes, **Docker containers on the same host** can communicate with each other by default. This is because, when you run a Docker container, it's on a single **network namespace of the host**, and Docker uses that network namespace to manage communication between containers.

#### Default Network Configuration

By default, Docker provides each container with its own network stack. The configuration includes:

- **IP Address**: Obtained from the Docker network.
- **Network Interfaces**: Namespaced within the container.

#### Default Docker Bridge Network

A Docker bridge network, such as `docker0`, serves as the default network type. Containers within the same bridge network can communicate with each other by their **container names or IP addresses**.

#### Custom Networks

Containers can also be part of user-defined bridge networks or other network types. In such configurations, containers belonging to the same network can communicate with each other.

### Configuring Communication

Direct container-to-container communication is straightforward. Once a container knows the other's IP address, it can initiate communication.

Here are two key methods to configure container communication:

#### 1. By Container IP

```bash
docker inspect -f '{{.NetworkSettings.IPAddress}}' <container_id>
```

#### 2. By Container Name

Containers within the same Docker network can reach each other by their names. Use `docker network inspect <your_network_name>` to see container IP addresses and ensure proper network setup.
<br>


# docker-cheat-sheet
Quick reference guide for Docker commands

### Table of Contents

| No. | Questions |
|---- | ---------
|1  | [**What is docker?**](#what-is-docker) |
|2  | [**Why docker?**](#why-docker)|
|3  | [**Installation**](#installation) |
|4  | [**Registries and Repositories**](#registries-and-repositories)|
|5  | [**Create,Run,Update and Delete containers**](#)|
|6  | [**Start and stop containers**](#start-and-stop-containers) |
|7  | [**Networks**](#networks)|
|8  | [**Cleanup commands**](#cleanup-commands)|
|9  | [**Utility commands**](#utility-commands)|
|10 | [**Docker Hub**](#docker-hub)|
|11 | [**Dockerfile**](#dockerfile)|
|12 | [**Docker Compose**](#docker-compose)|
|13 | [**Docker Swarm**](#docker-swarm)|

### What is docker?
   Docker is a tool designed to make it easier to create, deploy, and run applications by using containers.

   **[⬆ Back to Top](#table-of-contents)**

### Why docker?
   Docker is useful to automate the deployment of applications inside a software containers, which makes the applications easy to ship and run virtually anywhere (i.e, platform independent). The Docker container processes run on the host kernel, unlike VM which runs processes in guest kernel.

   ![dockervsvm](images/dockervsvm.jpg)

   **[⬆ Back to Top](#table-of-contents)**

### Installation
The docker desktop downloads are available for windows, mac and linux distributions.

#### Windows
It supports for Windows 10 64-bit: Home, Pro, Enterprise, or Education, version 1903 (Build 18362 or higher). You need to follow the below steps for installation.

1. Download docker desktop for windows from https://docs.docker.com/docker-for-windows/install/
2. Double-click `Docker Desktop Installer.exe` to run the installer.
3. Make sure `Enable Hyper-V Windows Features` option is selected

#### Mac

1. Download docker desktop for mac from https://docs.docker.com/docker-for-mac/install/
2. Double-click `Docker.dmg` to open the installer and drag it to the Applications folder.
3. Double-click `Docker.app` in the Applications folder to start Docker.

#### Linux

You can install from a package easily
1. Go to https://download.docker.com/linux/ubuntu/dists/, choose your Ubuntu version and then go to pool/stable/ to get .deb file
2. Install Docker Engine by referring the downloaded location of the Docker package.
```cmd
$ sudo dpkg -i /path/to/package.deb
```
3. Verify the Docker Engine by running the `hello-world` image to check correct installation.
```cmd
$ sudo docker run hello-world
```

   **[⬆ Back to Top](#table-of-contents)**

### Registries and Repositories
#### Registry:
Docker Registry is a service that stores your docker images. It could be hosted by a third party, as public or private registry. Some of the examples are,

- Docker Hub,
- Quay,
- Google Container Registry,
- AWS Container Registry

#### Repository:
A Docker Repository is a collection of related images with same name which have different tags. These tags are an alphanumeric identifiers(like 1.0 or latest) attached to images within a repository.

For example, if you want to pull golang image using `docker pull golang:latest` command, it will download the image tagged latest within the `golang` repository from the Docker Hub registry. The tags appeared on dockerhub as below,

#### Login
Login to a registry
```js
> docker login [OPTIONS] [SERVER]

[OPTIONS]:
-u/--username username
-p/--password password

Example:

1. docker login localhost:8080 // Login to a registry on your localhost
2. docker login
```

#### Logout
Logout from a registry
```js
> docker logout [SERVER]

Example:

docker logout localhost:8080 // Logout from a registry on your localhost
```

#### Search image
Search for an image in registry
```js
docker search [OPTIONS] TERM

Example:
docker search golang
docker search --filter stars=3 --no-trunc golang
```
#### Pull image

This command pulls an image or a repository from a registry to local machine

```js
docker image pull [OPTIONS] NAME[:TAG|@DIGEST]

Example:
docker image pull golang:latest
```
#### Push image

This command pushes an image to the registry from local machine.

```js
docker image push [OPTIONS] NAME[:TAG]
docker image push golang:latest
```

  **[⬆ Back to Top](#table-of-contents)**

### Create,Run,Update and Delete containers

#### Create
Create a new container
```cmd
docker container create [OPTIONS] IMAGE [COMMAND] [ARG...]

Example:
docker container create -t -i sudheerj/golang --name golang
```

#### Rename

Rename a container

```cmd
docker container rename CONTAINER NEW_NAME

Example:
docker container rename golang golanguage
docker container rename golanguage golang
```

#### Run

```cmd
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

Example:
docker container run -it --name golang -d sudheerj/golang
```

You can also run a command inside container
```cmd
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Example:
docker exec -it golang sh // Or use bash command if sh is failed
```

#### Update
Update configuration of one or more containers

```cmd
docker container update [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container update --memory "1g" --cpuset-cpu "1" golang // update the golang to use 1g of memory and only use cpu core 1
```

#### Remove
Remove one or more containers

```cmd
docker container rm [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container rm golang
docker rm $(docker ps -q -f status=exited) // Remove all the stopped containers
```
  **[⬆ Back to Top](#table-of-contents)**
### Start and stop containers

#### Start
Start one or more stopped containers

```cmd
docker container start [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container start golang
```

### Stop
Stop one or more running containers


```cmd
docker container stop [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container stop golang
docker stop $(docker ps -a -q) // To stop all the containers
```

#### Restart
Restart one or more containers and processes running inside the container/containers.

```cmd
docker container restart [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container restart golang
```

#### Pause
Pause all processes within one or more containers

```cmd
docker container pause CONTAINER [CONTAINER...]

Example:
docker container pause golang
```

### Unpause/Resume
Unpause all processes within one or more containers

```cmd
docker container unpause CONTAINER [CONTAINER...]

Example:
docker container unpause golang
```

#### Kill
Kill one or more running containers

```cmd
docker container kill [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container kill golang
```

#### Wait
Block until one or more containers stop and print their exit codes after that


```cmd
docker container wait CONTAINER [CONTAINER...]

Example:
docker container wait golang
```
  **[⬆ Back to Top](#table-of-contents)**

### Networks
   Docker provides network commands connect containers to each other and to other non-Docker workloads. The usage of network commands would be `docker network COMMAND`

#### List networks
List down available networks

```cmd
docker network ls
```

#### Connect a container to network
You can connect a container by name or by ID to any network. Once it connected, the container can communicate with other containers in the same network.

```cmd
docker network connect [OPTIONS] NETWORK CONTAINER

Example:
docker network connect multi-host-network container1
```

#### Disconnect a container from a network
You can disconnect a container by name or by ID from any network.

```cmd
docker network disconnect [OPTIONS] NETWORK CONTAINER

Example:
docker network disconnect multi-host-network container1
```

#### Remove one or more networks
Removes one or more networks by name or identifier. Remember, you must first disconnect any containers connected to it before removing it.

```cmd
docker network rm NETWORK [NETWORK...]

Example:
docker network rm my-network
```

#### Create network
It is possible to create a network in Docker before launching containers

```cmd
docker network create [OPTIONS] NETWORK

Example:
sudo docker network create –-driver bridge some_network
```

The above command will output the long ID for the new network.

#### Inspect network
You can see more details on the network associated with Docker using network inspect command.

```cmd
docker network inspect networkname

Example:
docker network inspect bridge
```
### Cleanup commands

You may need to cleanup resources (containers, volumes, images, networks) regularly.

#### Remove all unused resources

```cmd
docker system prune
```

#### Images

```cmd
$ docker images
$ docker rmi $(docker images --filter "dangling=true" -q --no-trunc)

$ docker images | grep "none"
$ docker rmi $(docker images | grep "none" | awk '/ / { print $3 }')
```

#### Containers

```cmd
$ docker ps
$ docker ps -a
$ docker rm $(docker ps -qa --no-trunc --filter "status=exited")
```

#### Volumes

```cmd
$ docker volume rm $(docker volume ls -qf dangling=true)
$ docker volume ls -qf dangling=true | xargs -r docker volume rm
```

#### Networks

```cmd
$ docker network ls
$ docker network ls | grep "bridge"
$ docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
```

  **[⬆ Back to Top](#table-of-contents)**

### Utility commands

  **[⬆ Back to Top](#table-of-contents)**

### Docker Hub
   Docker Hub is a cloud-based repository provided by Docker to test, store and distribute container images which can be accessed either privately or publicly.

#### From
   It initializes a new image and sets the Base Image for subsequent instructions. It must be a first non-comment instruction in the Dockerfile.
   ```cmd
   FROM <image>
   FROM <image>:<tag>
   FROM <image>@<digest>
   ```
   **Note:** Both `tag` and `digest` are optional. If you omit either of them, the builder assumes a latest by default.

### Dockerfile
   Dockerfile is a text document that contains set of commands and instructions which will be executed in a sequence in the docker environment for building a new docker image.

#### FROM
This command Sets the Base Image for subsequent instructions

```cmd
FROM <image>
FROM <image>:<tag>
FROM <image>@<digest>

Example:
FROM ubuntu:18.04
```

#### RUN

RUN instruction allows you to install your application and packages required for it. It executes any commands on top of the current image and creates a new layer by committing the results. It is quite common to have multiple RUN instructions in a Dockerfile.

It has two forms
1. Shell Form: RUN <command>
```cmd
RUN npm start
```
2. Exec form RUN ["<executable>", "<param1>", "<param2>"]
```cmd
RUN [ "npm", "start" ]
```

#### ENTRYPOINT
An ENTRYPOINT allows you to configure a container that will run as an executable. It is used to run when container starts.

```cmd
Exec Form:
ENTRYPOINT ["executable", "param1", "param2"]
Shell Form:
ENTRYPOINT command param1 param2

Example:
FROM alpine:3.5
ENTRYPOINT ["/bin/echo", "Print ENTRYPOINT instruction of Exec Form"]

```

If an image has an ENTRYPOINT and pass an argument to it while running the container, it wont override the existing entrypoint but it just appends what you passed with the entrypoint. To override the existing ENTRYPOINT. you should user `–entrypoint` flag for the running container.

Let's see the behavior with the above dockerfile,

```cmd
Build image:
docker build -t entrypointImage .

Run the image:
docker container run entrypointImage // Print ENTRYPOINT instruction of Exec Form

Override entrypoint:
docker run --entrypoint "/bin/echo" entrypointImage "Override ENTRYPOINT instruction" // Override ENTRYPOINT instruction
```

#### CMD
CMD instruction is used to set a default command, which will be executed only when you run a container without specifying a command. But if the docker container runs with a command, the default command will be ignored.

The CMD instruction has three forms,
```cmd
1. Exec form:
CMD ["executable","param1","param2"]
2. Default params to ENTRYPOINT:
CMD ["param1","param2"]
3. Shell form:
CMD command param1 param2
```

The main purpose of the CMD command is to launch the required software in a container. For example, running an executable .exe file or a Bash terminal as soon as the container starts.

Remember, if docker runs with executable and parameters then CMD instruction will be overridden(Unlike ENTRYPOINT).

```cmd
docker run executable parameters
```

**Note:** There should only be one CMD command in your Dockerfile. Otherwise only the last instance of CMD will be executed.

#### COPY
The COPY instruction copies new files or directories from source and adds them to the destination filesystem of the container.

```cmd
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]

Example:
COPY test.txt /absoluteDir/
COPY tes? /absoluteDir/ // Copies all files or directories starting with test to destination container
```

The <src> path must be relative to the source directory that is being built. Whereas <dest> is an absolute path, or a path relative to `WORKDIR`.
#### ADD
The ADD instruction copies new files, directories or remote file URLs from source and adds them to the filesystem of the image at the destination path. The functionality is similar to COPY command and supports two forms of usage,

```cmd
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

Example:
ADD test.txt /absoluteDir/
ADD tes? /absoluteDir/ // Copies all files or directories starting with test to destination container
```

ADD commands provides additional features such as downloading remote resources, extracting TAR files etc.

```cmd
1. Download an external file and copy to the destination
ADD http://source.file/url  /destination/path

2. Copies compressed files and extract the content in the destination
ADD source.file.tar.gz /temp
```

#### ENV
The ENV instruction sets the environment variable <key> to the value <value>. It has two forms,

1. The first form, `ENV <key> <value>`, will set a single variable to a value.
2. The second form, `ENV <key>=<value> ...`, allows for multiple variables to be set at one time.

```cmd
ENV <key> <value>
ENV <key>=<value> [<key>=<value> ...]

Example:
ENV name="John Doe" age=40
ENV name John Doe
ENV age 40
```

#### EXPOSE
The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. i.e, It helps in inter-container communication. You can specify whether the port listens on TCP or UDP, and the default is TCP.

```cmd
EXPOSE <port> [<port>/<protocol>...]

Example:
EXPOSE 80/udp
EXPOSE 80/tcp
```

But if you want to bind the port of the container with the host machine on which the container is running, use -p option of `docker run` command.

```cmd
docker run -p <HOST_PORT>:<CONTAINER:PORT> IMAGE_NAME

Example:
docker run -p 80:80/udp myDocker
```

#### WORKDIR
The WORKDIR command is used to define the working directory of a Docker container at any given time for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.

```cmd
WORKDIR /path/to/workdir

Example:
WORKDIR /c
WORKDIR d
WORKDIR e
RUN pwd  // /c/d/e
```

#### LABEL
The LABEL instruction adds metadata as key-value pairs to an image. Labels included in base or parent images (images in the FROM line) are inherited by your image.

```cmd
LABEL <key>=<value> <key>=<value> <key>=<value> ...

Example:
LABEL version="1.0"
LABEL multi.label1="value1" \
      multi.label2="value2" \
      other="value3"
```

You can view an image’s labels using the `docker image inspect --format='' myimage` command. The output would be as below,

```js
{
  "version": "1.0",
  "multi.label1": "value1",
  "multi.label2": "value2",
  "other": "value3"
}
```

#### MAINTAINER
The MAINTAINER instruction sets the Author field of the generated images.
```cmd
MAINTAINER <name>

Example:
MAINTAINER John
```

This command is deprecated status now and the recommended usage is with LABEL command
```cmd
LABEL maintainer="John"
```

#### VOLUME
The VOLUME instruction creates a mount point with the specified name and mounted volumes from native host or other containers.

```cmd
VOLUME ["/data"]

Example:
FROM ubuntu
RUN mkdir /test
VOLUME /test
```

### Docker Compose
   Docker compose(or compose) is a tool for defining and running multi-container Docker applications.
### Docker Swarm
   Docker Swarm(or swarm) is an open-source tool used to cluster and orchestrate Docker containers.




# Docker Interview Questions

Docker is getting a lot of traction in the industry because of its performance-savvy and universal replicability architecture, while providing the following four cornerstones of modern application development: autonomy, decentralization, parallelism & isolation. 
Below are top 50 interview questions for candidates who want to prepare on Docker Container Technology:


# What are 5 similarities between Docker & Virtual Machine?

Docker is not quite like a VM. It uses the host kernel & can’t boot a different operating system. Below are 5 similarities between Docker & VIrtual Machine:
 
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/Picture1.png)

# How is Docker different from Virtual Machine?
 
Figure: Docker Vs VM
Below are list of 6 difference between Docker container & VM:
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview2.png)
 

# What is the difference between Container Networking & VM Networking?
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-3.png)



# Is it possible to run multiple process inside Docker container?
Yes, you can run multiple processes inside Docker container. This approach is discouraged for most use cases. For maximum efficiency and isolation, each container should address one specific area of concern. However, if you need to run multiple services within a single container, you can use tools like supervisor.
Supervisor is a moderately heavy-weight approach that requires you to package supervisord and its configuration in your image (or base your image on one that includes supervisord), along with the different applications it manages. Then you start supervisord, which manages your processes for you. 
Example: Here is a Dockerfile using this approach, that assumes the pre-written supervisord.conf, my_first_process, and my_second_process files all exist in the same directory as your Dockerfile.
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-4.png)
 

# Does Docker run on Linux, macOS and Windows?
You can run both Linux and Windows programs and executables in Docker containers. The Docker platform runs natively on Linux (on x86-64, ARM and many other CPU architectures) and on Windows (x86-64). Docker Inc. builds products that let you build and run containers on Linux, Windows and macOS.

#  What is DockerHub?

DockerHub is a cloud-based registry service which allows you to link to code repositories, build your images and test them, stores manually pushed images, and links to Docker cloud so you can deploy images to your hosts. It provides a centralized resource for container image discovery, distribution and change management, user and team collaboration, and workflow automation throughout the development pipeline.

# What is Dockerfile?

Docker builds images automatically by reading the instructions from a text file called Dockerfile. It contains all commands, in order, needed to build a given image. A Dockerfile adheres to a specific format and set of instructions which you can find here.

#  How is Dockerfile different from Docker Compose?

A Dockerfile is a simple text file that contains the commands a user could call to assemble an image whereas Docker Compose is a tool for defining and running multi-container Docker applications. Docker Compose define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment. It get an app running in one command by just running  docker-compose up.
Docker compose uses the Dockerfile if one add the build command to your project's docker-compose.yml. Your Docker workflow should be to build a suitable Dockerfile for each image you wish to create, then use compose to assemble the images using the build command.

# Can I use JSON instead of YAML for my Docker Compose file?
Yes. Yaml is a superset of json so any JSON file should be valid Yaml. To use a JSON file with Compose, specify the filename to use, for example:
docker-compose -f docker-compose.json up

You can use json instead of yaml for your compose file, to use json file with compose, specify the filename to use for eg:
docker-compose -f docker-compose.json up

# How to create Docker container?

We can use Docker image to create Docker container by using the below command:

```
$ docker run -t -i command name
```

This command will create and start a container.If you want to verify the list of all running container with the status on a host use the below command:
```
$ docker ps -a
```

# What is maximum number of container you can run per host?
This really depends on your environment. The size of your applications as well as the amount of available resources (i.e like CPU) will all affect the number of containers that can be run in your environment. Containers unfortunately are not magical. They can’t create new CPU from scratch. They do, however, provide a more efficient way of utilizing your resources. The containers themselves are super lightweight (remember, shared OS vs individual OS per container) and only last as long as the process they are running. 

# Is it possible to have my own private Docker registry?
Yes, it is possible today using Docker own registry server. if you want to use 3rd party tool, see Portus.
TBA

# Does Docker container package up the entire OS?
Docker containers do not package up the OS. They package up the applications with everything that the application needs to run. The engine is installed on top of the OS running on a host. Containers share the OS kernel allowing a single host to run multiple containers.

# Describe how many ways are available to configure Docker daemon?
There are two ways to configure the Docker daemon:
-	Using a JSON configuration file. 
This is the preferred option, since it keeps all configurations in a single place.
-	Using flags when starting dockerd.
You can use both of these options together as long as you don’t specify the same option both as a flag and in the JSON file. If that happens, the Docker daemon won’t start and prints an error message.
$ dockerd --debug   --tls=true  --tlscert=/var/docker/server.pem --tlskey=/var/docker/serverkey.pem \
  --host tcp://<Host_IP>:2376
15. Can you list reasons why Container Networking is so important?
Below are top 5 reasons why we need container networking:
-	Containers need to talk to external world. 
-	Reach Containers from external world to use the service that Containers provides. 
-	Allows Containers to talk to host machine. 
-	Inter-container connectivity in same host and across hosts. 
-	Discover services provided by containers automatically. 
-	Load balance traffic between different containers in a service.
-	Provide secure multi-tenant services.

# What does CNM refers to? What are its components? ![img](
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-5.png)
 
CNM refers to Container Networking Model. The Container Network Model (CNM) is a standard or specification from Docker, Inc. that forms the basis of container networking in a Docker environment.It is Docker’s approach to providing container networking with support for multiple network drivers. The CNM provides the following contract between networks and containers:
-	All containers on the same network can communicate freely with each other
-	Multiple networks are the way to segment traffic between containers and should be supported by all drivers
-	Multiple endpoints per container are the way to join a container to multiple networks
-	An endpoint is added to a network sandbox to provide it with network connectivity

The major components of the CNM are:
-	 Network, 
-	Sandbox and 
-	Endpoint.
Sandbox is a generic term that refers to OS specific technologies used to isolate networks stacks on a Docker host. Docker on Linux uses kernel namespaces to provide this sandbox functionality. Networks “stacks” inside of sandboxes include interfaces, routing tables, DNS etc. A network in CNM terms is one or more endpoints that can communicate.All endpoints on the same network can communicate with each other.Endpoints on different networks cannot communicate without external routing.
  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-6.png)

# What are different types of Docker Networking drivers? 
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-7.png)

Docker’s networking subsystem is pluggable using drivers. Several drivers exist by default, and provide core networking functionality. Below is the snapshot of difference of various Docker networking drivers.

 



Below are details of Docker networking drivers:
Bridge: The default network driver. If you don’t specify a driver, this is the type of network you are creating. Bridge networks are usually used when your applications run in standalone containers that need to communicate. 

Host: For standalone containers, remove network isolation between the container and the Docker host, and use the host’s networking directly. host is only available for swarm services on Docker 17.06 and higher. 

Overlay: Overlay networks connect multiple Docker daemons together and enable swarm services to communicate with each other. You can also use overlay networks to facilitate communication between a swarm service and a standalone container, or between two standalone containers on different Docker daemons. This strategy removes the need to do OS-level routing between these containers. See overlay networks.

MacVLAN: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host’s network stack. 

None: For this container, disable all networking. Usually used in conjunction with a custom network driver. none is not available for swarm services. 


# What features are possible only under Docker Enterprise Edition in comparison to Docker Community Edition?
 The following two features are only possible when using Docker EE and managing your Docker services using Universal Control Plane (UCP):
The HTTP routing mesh allows you to share the same network IP address and port among multiple services. UCP routes the traffic to the appropriate service using the combination of hostname and port, as requested from the client.
Session stickiness allows you to specify information in the HTTP header which UCP uses to route subsequent requests to the same service task, for applications which require stateful sessions.


# How is Docker Bridge network different from traditional Linux bridge ?
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-8.png)
 
In terms of networking, a bridge network is a Link Layer device which forwards traffic between network segments. A bridge can be a hardware device or a software device running within a host machine’s kernel.
In terms of Docker, a bridge network uses a software bridge which allows containers connected to the same bridge network to communicate, while providing isolation from containers which are not connected to that bridge network. The Docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks cannot communicate directly with each other.

# How to create a user-defined Bridge network ?
To create a user-defined bridge network, one can use the docker network create command - 

```$ docker network create mynet```

![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-9.png)
 
You can specify the subnet, the IP address range, the gateway, and other options. See the docker network create reference or the output of docker network create --help for details.

# How to delete a user-defined Bridge network ?
Use the docker network rm command to remove a user-defined bridge network. If containers are currently connected to the network, disconnect them first.

```$ docker network rm mynet```
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-10.png)

 
# How to connect Docker container to user-defined bridge network?

![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-11.png)
 
When you create a new container, you can specify one or more --network flags. This example connects a Nginx container to the my-net network. It also publishes port 80 in the container to port 8080 on the Docker host, so external clients can access that port. Any other container connected to the my-net network has access to all ports on the my-nginx container, and vice versa.
```
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
```
To connect a running container to an existing user-defined bridge, use the docker network connect command. The following command connects an already-running my-nginx container to an already-existing my-net network:
```
$ docker network connect my-net my-nginx

```



# Does Docker support IPv6?

![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-12.png)

 
Yes, Docker support IPv6.  IPv6 networking is only supported on Docker daemons running on Linux hosts.Support for IPv6 address has been there since Docker Engine 1.5 release.To enable IPv6 support in the Docker daemon, you need to edit ```/etc/docker/daemon.json ```and set the ipv6 key to true.
```
{
  "ipv6": true
}
```
Ensure that you reload the Docker configuration file.
```
$ systemctl reload docker
```
You can now create networks with the` --ipv6 `flag and assign containers IPv6 addresses using the `--ip6` flag.

# Does Docker Compose file format support IPv6 protocol?
Yes.

# How is overlay network different from bridge network?
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-13.png)
 
Bridge networks connect two networks while creating a single aggregate network from multiple communication networks or network segments, hence the name bridge.
Overlay networks are usually used to create a virtual network between two separate hosts. Virtual, since the network is build over an existing network.
Bridge networks can cater to single host, while overlay networks are for multiple hosts.

26. What networks are affected when you join a Docker host to an existing Swarm?
When you initialize a swarm or join a Docker host to an existing swarm, two new networks are created on that Docker host:
-	an overlay network called ingress, which handles control and data traffic related to swarm services. When you create a swarm service and do not connect it to a user-defined overlay network, it connects to the ingress network by default.
-	a bridge network called docker_gwbridge, which connects the individual Docker daemon to the other daemons participating in the swarm.

# How shall you disable the networking stack on a container?
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-14.png)
If you want to completely disable the networking stack on a container, you can use the --network none flag when starting the container. Within the container, only the loopback device is created. The following example illustrates this.
 

# How can one create MacVLAN network for Docker container?
To create a Macvlan network which bridges with a given physical network interface, once can use --driver macvlan with the docker network create command. You also need to specify the parent, which is the interface the traffic will physically go through on the Docker host.
```
$ docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1  \
  -o parent=eth0 collabnet
```

# Is it possible to exclude IP address from being used in MacVLAN network?
If you need to exclude IP addresses from being used in the Macvlan network, such as when a given IP address is already in use, use ```--aux-addresses```:
```
$ docker network create -d macvlan  \
  --subnet=192.168.32.0/24  \
  --ip-range=192.168.32.128/25 \
  --gateway=192.168.32.254  \
  --aux-address="my-router=192.168.32.129" \
  -o parent=eth0 collabnet32
```
# Do I lose my data when the container exits?
Not at all! Any data that your application writes to disk gets preserved in its container until you explicitly delete the container. The file system for the container persists even after the container halts.
# Does Docker Enterprise Edition support Kubernetes?
Yes, Docker Enterprise Edition(rightly called EE) support Kubernetes. EE 2.0 allows users to choose either Kubernetes or Swarm at the orchestration layer.
# What is Docker Swarm?
Docker Swarm is native clustering for Docker. It turns a pool of Docker hosts into a single, virtual Docker host. Docker Swarm serves the standard Docker API, any tool that already communicates with a Docker daemon can use Swarm to transparently scale to multiple hosts.




# What is `--memory-swap` flag?
`--memory-swap` is a modifier flag that only has meaning if `--memory `is also set. Using swap allows the container to write excess memory requirements to disk when the container has exhausted all the RAM that is available to it. There is a performance penalty for applications that swap memory to disk often.

# Can you explain different volume mount types  available in Docker?
There are three mount types available in Docker   	
· Volumes are stored in a part of the host filesystem which is managed by Docker (`/var/lib/docker/volumes/` on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.
·  Bind mounts may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.
·   tmpfs mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.
 
#  How to share data among DockerHost?
Ways to achieve this when developing your applications. One is to add logic to your application to store files on a cloud object storage system like Amazon S3. Another is to create volumes with a driver that supports writing files to an external storage system like NFS or Amazon S3.
Volume drivers allow you to abstract the underlying storage system from the application logic. For example, if your services use a volume with an NFS driver, you can update the services to use a different driver, as an example to store data in the cloud, without changing the application logic.
 
# How to Backup, Restore, or Migrate data volumes under Docker container?
 
Steps to Backup a container
1)      Launch a new container and mount the volume from the dbstore container
2)      Mount a local host directory as /backup
3)      Pass a command that tars the contents of the dbdata volume to a backup.tar file inside our /backup directory.
`$ docker run --rm --volumes-from dbstore -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata`
 Restore container from backup
With the backup just created, you can restore it to the same container, or another that you made elsewhere.
For example, create a new container named dbstore2:
`$ docker run -v /dbdata --name dbstore2 ubuntu /bin/bash`

Then un-tar the backup file in the new container`s data volume:
```
$ docker run --rm --volumes-from dbstore2 -v $(pwd):/backup ubuntu bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1
```

#  How to Configure Automated Builds on DockerHub
You can build your images automatically from a build context stored in a repository. A build context is a Dockerfile and any files at a specific location. For an automated build, the build context is a repository containing a Dockerfile. 

# How to configure the default logging driver under Docker?

To configure the Docker daemon to default to a specific logging driver, set the value of log-driver to the name of the logging driver in the daemon.json file, which is located in /etc/docker/ on Linux hosts or C:\ProgramData\docker\config\ on Windows server hosts. The default logging driver is json-file.


# Why do my services take 10 seconds to recreate or stop?

Compose stop attempts to stop a container by sending a SIGTERM. It then waits for a default timeout of 10 seconds. After the timeout, a SIGKILL is sent to the container to forcefully kill it. If you are waiting for this timeout, it means that your containers aren’t shutting down when they receive the SIGTERM signal.

# How do I run multiple copies of a Compose file on the same host?

Compose uses the project name to create unique identifiers for all of a project’s containers and other resources. To run multiple copies of a project, set a custom project name using the -command line option or the COMPOSE_PROJECT_NAME environment variable.

# What’s the difference between up, run, and start under Docker Compose?

Typically, you want docker-compose up. Use up to start or restart all the services defined in a docker-compose.yml. In the default “attached” mode, you see all the logs from all the containers. In “detached” mode (-d), Compose exits after starting the containers, but the containers continue to run in the background.

The docker-compose run command is for running “one-off” or “adhoc” tasks. It requires the service name you want to run and only starts containers for services that the running service depends on. Use run to run tests or perform an administrative task such as removing or adding data to a data volume container. The run command acts like docker run -ti in that it opens an interactive terminal to the container and returns an exit status matching the exit status of the process in the container.
The docker-compose start command is useful only to restart containers that were previously created, but were stopped. It never creates new containers.

#  What is Docker Trusted Registry?
Docker Trusted Registry (DTR) is the enterprise-grade image storage solution from Docker. You install it behind your firewall so that you can securely store and manage the Docker images you use in your applications.

# How to declare default environment variables under Docker Compose?


Compose supports declaring default environment variables in an environment file named .env placed in the folder where the docker-compose command is executed (current working directory).
Example: The below example demonstrate how to declare default environmental variable for Docker Compose.
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-16.png)

 
When you run docker-compose up, the web service defined above uses the image alpine:v3.4. You can verify this with the `docker-compose config` command which prints your resolved application config to the terminal:
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-17.png)

 

# Can you list out ways to share Compose configurations between files and projects under Docker Compose?

Compose supports two methods of sharing common configuration:
1.	Extending an entire Compose file by using multiple Compose files
2.	Extending individual services with the extends field

# What is the role of .dockerignore file?
To understand the role of .dockerignore file, let us take a practical example. You may have noticed that if you put a Dockerfile in your home directory and launch a docker build you will see a message uploading context. Right? This means docker creates a .tar with all the files in your home and in all the subdirectories, and uploads this tar to the docker daemon. If you have some huge files, this may take a long time.
In order to avoid this, you might need to create a specific directory, where you put your Dockerfile, and all what is needed for your build. It becomes necessary to tell docker to ignore some files during the build. Hence, you need to put in the .dockerignore all the files not needed for your build
Before the docker CLI sends the context to the docker daemon, it looks for a file named .dockerignore in the root directory of the context. If this file exists, the CLI modifies the context to exclude files and directories that match patterns in it. This helps to avoid unnecessarily sending large or sensitive files and directories to the daemon and potentially adding them to images using ADD or COPY.

# What is the purpose of EXPOSE command in Dockerfile?
When writing your Dockerfiles, the instruction EXPOSE tells Docker the running container listens on specific network ports. This acts as a kind of port mapping documentation that can then be used when publishing the ports.

`EXPOSE <port> [<port>/<protocol>...]`
![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-18.png)
 
You can also specify this within a docker run command, such as:

`docker run --expose=1234 my_app`

Please note that EXPOSE will not allow communication via the defined ports to containers outside of the same network or to the host machine. To allow this to happen you need to publish the ports.

# How is ENTRYPOINT instruction under Dockerfile different from RUN instruction?
ENTRYPOINT is meant to provide the executable while CMD is to pass the default arguments to the executable.
To understand it clearly, let us consider the below Dockerfile:
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-19.png)

If you try building this Docker image using `docker build command` -

  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-21.png)

 Let us run this image without any argument.
  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-22.png)

 

Let's run it passing a command line argument
  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-23.png)
 
This clearly state that ENTRYPOINT is meant to provide the executable while CMD is to pass the default arguments to the executable.
# Why Build cache in Docker is so important? 
If the objects on the file system that Docker is about to produce are unchanged between builds, reusing a cache of a previous build on the host is a great time-saver. It makes building a new container really, really fast. None of those file structures have to be created and written to disk this time — the reference to them is sufficient to locate and reuse the previously built structures.
# Why Docker Monitoring is necessary?
●	Monitoring helps to identify issues proactively that would help to avoid system outages.
●	The monitoring time-series data provide insights to fine-tune applications for better performance and robustness.
●	With full monitoring in place, changes could be rolled out safely as issues will be caught early on and be resolved quickly before that transforms into root-cause for an outage.
●	The changes are inherent in container based environments and impact of that too gets monitored indirectly.
 

# Difference between Windows Containers and Hyper-V Containers
  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-24.png)

Underlying is the architecture laid out by the Microsoft for the Windows and Hyper-V Containers
 
Here are few of the differences between them,
Differences:
  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-25.png)
 

# What are main difference between Swarm & Kubernetes?
Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications. It  was built by Google based on their experience running containers in production using an internal cluster management system called Borg (sometimes referred to as Omega). In the other hand, a Swarm cluster consists of Docker Engine deployed on multiple nodes. Manager nodes perform orchestration and cluster management. Worker nodes receive and execute tasks 
Below are the major list of differences between Docker Swarm & Kubernetes:
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-26.png)
 

 	 
Applications are deployed in the form of  services (or “microservices”) in a Swarm cluster. Docker Compose is a tool which is majorly  used to deploy the app. 	Applications are deployed in the form of a combination of pods, deployments, and services (or “microservices”). 
Autoscaling feature is not available either in  Docker Swarm (Classical) or Docker Swarm 	Auto-scaling feature is available  under K8s. It   uses a simple number-of-pods target which is defined declaratively using deployments. CPU-utilization-per-pod target is available. 


Docker Swarm support rolling updates features. At rollout time, you can apply rolling updates to services. The Swarm manager lets you control the delay between service deployment to different sets of nodes, thereby updating only 1 task at a time.	Under kubernetes, the deployment controller  supports both “rolling-update” and “recreate” strategies. Rolling updates can  specify maximum number of pods unavailable or maximum number running during the process.
Under Docker Swarm Mode, the node joining a Docker Swarm cluster creates an overlay network for services that span all of the hosts in the Swarm and a host only Docker bridge network for containers.
By default, nodes in the Swarm cluster encrypt overlay control and management traffic between themselves. Users can choose to encrypt container data traffic when creating an overlay network by themselves.
	Under K8s, the networking model is a flat network, enabling all pods to communicate with one another. Network policies specify how pods communicate with each other. The flat network is typically implemented as an overlay.

Docker Swarm health checks are limited to services. If a container backing the service does not come up (running state), a new container is kicked off.
Users can embed health check functionality into their Docker images using the HEALTHCHECK instruction.
	Under K8s, the health checks are of two kinds: liveness (is app responsive) and readiness (is app responsive, but busy preparing and not yet able to serve)
Out-of-the-box K8S provides a basic logging mechanism to pull aggregate logs for a set of containers that make up a pod.




#  Is it possible to run Kubernetes on Docker EE 2.0 Platform?
Yes, it is possible to run Kubernetes under Docker EE 2.0 platform. Docker Enterprise Edition (EE) 2.0 is the only platform that manages and secures applications on Kubernetes in multi-Linux, multi-OS and multi-cloud customer environments. As a complete platform that integrates and scales with your organization, Docker EE 2.0 gives you the most flexibility and choice over the types of applications supported, orchestrators used, and where it’s deployed. It also enables organizations to operationalize Kubernetes more rapidly with streamlined workflows and helps you deliver safer applications through integrated security solutions.

# Can you use Docker Compose to build up Swarm/Kubernetes Cluster?
Yes, one can deploy a stack on Kubernetes with docker stack deploy command, the docker-compose.yml file, and the name of the stack.
Example:
$docker stack deploy --compose-file /path/to/docker-compose.yml mystack
                   $docker stack services mystack
You can see the service deployed with the kubectl get services command
                  $kubectl get svc,po,deploy

# What is 'docker stack deploy' command meant for?
The ‘docker stack deploy’ is a command to deploy a new stack or update an existing stack. A stack is a collection of services that make up an application in a specific environment. A stack file is a file in YAML format that defines one or more services, similar to a docker-compose.yml file for Docker Compose but with a few extensions. 
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-27.png)
 
# List down major components of Docker EE 2.0?
Docker EE is more than just a container orchestration solution; it is a full lifecycle management solution for the modernization of traditional applications and microservices across a broad set of infrastructure platforms. It is a Containers-as-a-Service(CaaS) platform for IT that manages and secures diverse applications across disparate infrastructure, both on-premises and in the cloud. Docker EE provides an integrated, tested and certified platform for apps running on enterprise Linux or Windows operating systems and Cloud providers. It is tightly integrated to the underlying infrastructure to provide a native, easy to install experience and an optimized Docker environment.
Docker EE 2.0 GA consists of 3 major components which together enable a full software supply chain, from image creation, to secure image storage, to secure image deployment.
●	Universal Control Plane 3.0.0 (application and cluster management) – Deploys applications from images, by managing orchestrators, like Kubernetes and Swarm. UCP is designed for high availability (HA). You can join multiple UCP manager nodes to the cluster, and if one manager node fails, another takes its place automatically without impact to the cluster.
●	Docker Trusted Registry 2.5.0 – The production-grade image storage solution from Docker &
●	EE Engine 17.06.2- The commercially supported Docker engine for creating images and running them in Docker containers.

# Explain the concept of HA under Swarm Mode?
HA refers to High Availability. High Availability is a feature where you have multiple instances of your applications running in parallel to handle increased load or failures. These two paradigms fit perfectly into Docker Swarm, the built-in orchestrator that comes with Docker. Deploying your applications like this will improve your uptime which translates to happy users.
For creating a high availability container in the Docker Swarm, we need to deploy a docker service to the swarm with nginx image. This can be done by using docker swarm create command as specified above.
# docker service create --name nginx --publish 80:80 nginx
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-29.png)
 
# Can you explain what is Routing Mesh under Docker Swarm Mode?
Routing Mesh is a feature which make use of Load Balancer concepts.It provides global publish port for a given service. The routing mesh uses port based service discovery and load balancing. So to reach any service from outside the cluster you need to expose ports and reach them via the Published Port.
Docker Engine swarm mode makes it easy to publish ports for services to make them available to resources outside the swarm. All nodes participate in an ingress routing mesh. The routing mesh enables each node in the swarm to accept connections on published ports for any service running in the swarm, even if there’s no task running on the node. The routing mesh routes all incoming requests to published ports on available nodes to an active container.
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-30.png)
 
# Is Routing Mesh a Load Balancer? 
Routing Mesh is not Load-Balancer. It makes use of LB concepts.It provides global publish port for a given service. The routing mesh uses port based service discovery and load balancing. So to reach any service from outside the cluster you need to expose ports and reach them via the Published Port.
In simple words, if you had 3 swarm nodes, A, B and C, and a service which is running on nodes A and C and assigned node port 30000, this would be accessible via any of the 3 swarm nodes on port 30000 regardless of whether the service is running on that machine and automatically load balanced between the 2 running containers. I will talk about Routing Mesh in separate blog if time permits.

# Is it possible to run MacVLAN under Docker Swarm Mode? What features does it offer?
Starting Docker CE 17.06 release, Docker provides support for local scope networks in Swarm. This includes any local scope network driver. Some examples of these are bridge, host, and macvlan though any local scope network driver, built-in or plug-in, will work with Swarm. Previously only swarm scope networks like overlay were supported. 
 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-31.png)
 
MACVLAN offers a number of unique features and capabilities. It has positive performance implications by virtue of having a very simple and lightweight architecture. It’s use cases includes very low latency applications and networking design that requires containers be on the same subnet as and using IPs as the external host network.The macvlan driver uses the concept of a parent interface. This interface can be a physical interface such as eth0, a sub-interface for 802.1q VLAN tagging like eth0.10 (.10 representing VLAN 10), or even a bonded host adaptor which bundles two Ethernet interfaces into a single logical interface.







# What are Docker secrets and why is it necessary
In Docker there are three key components to container security and together they result in inherently safer apps.
  ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-32.png)
Docker Secrets, a container native solution that strengthens the Trusted Delivery component of container security by integrating secret distribution directly into the container platform.
By integrating secrets into Docker orchestration, we are able to deliver a solution for the secrets management problem that follows these exact principles.
The following diagram provides a high-level view of how the Docker swarm mode architecture is applied to securely deliver a new type of object to our containers: a secret object.
 

 ![img](https://raw.githubusercontent.com/collabnix/dockerlabs/master/docker/img/docker-interview-33.png)



.











# Serverless Interview Questions

## What is Serverless and why is it important?

Serverless allows you to build and run applications and services without thinking about servers. It eliminates infrastructure management tasks such as server or cluster provisioning, patching, operating system maintenance, and capacity provisioning. You can build them for nearly any type of application or backend service, and everything required to run and scale your application with high availability is handled for you.

## Why use serverless?

Serverless enables you to build modern applications with increased agility and lower total cost of ownership. Building serverless applications means that your developers can focus on their core product instead of worrying about managing and operating servers or runtimes, either in the cloud or on-premises. This reduced overhead lets developers reclaim time and energy that can be spent on developing great products which scale and that are reliable.

## What are the benefits of serverless?

- NO SERVER MANAGEMENT

There is no need to provision or maintain any servers. There is no software or runtime to install, maintain, or administer


- FLEXIBLE SCALING

Your application can be scaled automatically or by adjusting its capacity through toggling the units of consumption (e.g. throughput, memory) rather than units of individual servers.

- PAY FOR VALUE

Pay for consistent throughput or execution duration rather than by server unit.

- AUTOMATED HIGH AVAILABILITY

Serverless provides built-in availability and fault tolerance. You don't need to architect for these capabilities since the services running the application provide them by default.

# Tell something about the AWS Serverless Platform?

AWS provides a set of fully managed services that you can use to build and run serverless applications. Serverless applications don’t require provisioning, maintaining, and administering servers for backend components such as compute, databases, storage, stream processing, message queueing, and more. You also no longer need to worry about ensuring application fault tolerance and availability. Instead, AWS handles all of these capabilities for you. This allows you to focus on product innovation while enjoying faster time-to-market.

## COMPUTE

### AWS Lambda

AWS Lambda lets you run code without provisioning or managing servers. You pay only for the compute time you consume - there is no charge when your code is not running.

### AWS Fargate

AWS Fargate is a purpose-built serverless compute engine for containers. Fargate scales and manages the infrastructure required to run your containers.


## STORAGE

Amazon Simple Storage Service (Amazon S3) provides developers and IT teams with secure, durable, highly-scalable object storage. Amazon S3 is easy to use, with a simple web service interface to store and retrieve any amount of data from anywhere on the web.

## Amazon Elastic File System (Amazon EFS) 

It provides simple, scalable, elastic file storage. It is built to elastically scale on demand, growing and shrinking automatically as you add and remove files. 

## DATA STORES

Amazon DynamoDB is a fast and flexible NoSQL database service for all applications that need consistent, single-digit millisecond latency at any scale.

## API PROXY

Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. It offers a comprehensive platform for API management. API Gateway allows you to process hundreds of thousands of concurrent API calls and handles traffic management, authorization and access control, monitoring, and API version management.

## APPLICATION INTEGRATION

Amazon SNS is a fully managed pub/sub messaging service that makes it easy to decouple and scale microservices, distributed systems, and serverless applications.

## ORCHESTRATION

AWS Step Functions makes it easy to coordinate the components of distributed applications and microservices using visual workflows. Building applications from individual components that each perform a discrete function lets you scale and change applications quickly. Step Functions is a reliable way to coordinate components and step through the functions of your application.

## ANALYTICS

Amazon Kinesis is a platform for streaming data on AWS, offering powerful services to make it easy to load and analyze streaming data, and also providing the ability for you to build custom streaming data applications for specialized needs.

## DEVELOPER TOOLING

AWS provides tools and services that aid developers in the serverless application development process. AWS and its partner ecosystem offer tools for continuous integration and delivery, testing, deployments, monitoring and diagnostics, SDKs, frameworks, and integrated development environment (IDE) plugins.


# DCA Mock questions

## 1. How can we limit the number of CPUs provided to a container?

a) Using `--cap-add CPU` . <br>
b) Using` --cpuset-cpus` . <br>
c) Using` --cpus `. <br>
d) It is not possible to specify the number of CPUs;we have to use `--cpu-shares` and define the CPU slices. <br>


## 2. How can we limit the amount of memory available to a container?
a) It is not possible to limit the amount of memory available to a container.<br>
b) Using `--cap-drop MEM `.<br>
c) Using `--memory` .<br>
d) Using `--memory-reservation` .<br>

## 3.What environment variables should be exported to start using a trusted environment with the Docker client?
a) `export DOCKER_TRUSTED_ENVIRONMENT=1 `<br>
b) `export DOCKER_CONTENT_TRUST=1`<br>
c) `export DOCKER_TRUST=1`<br>
d) `export DOCKER_TRUSTED=1`<br>



# Deploying a Spring Boot application using Docker and running the Docker container on AWS EC2 instance 

Deployment process involves multiple steps, including Dockerizing the Spring Boot application, building a Docker image, running it locally, pushing it to the EC2 instance, and finally running the Docker container on EC2. Here’s a comprehensive guide:

### **Step 1: Dockerize Your Spring Boot Application**

1.  **Build the Spring Boot Application**:
    
    -   Navigate to your Spring Boot project directory.
        
    -   Use **Maven** or **Gradle** to package your application into a `jar` file:
      
        `mvn clean package` 
        
    -   This should generate a `jar` file in the `target` directory (e.g., `your-app.jar`).
        
2.  **Create a Dockerfile**:
    
    -   In the root directory of your project, create a file named `Dockerfile` (no extension).
        
    -   Example `Dockerfile`:
     
```
FROM maven:3.8.5-openjdk-11 AS build
WORKDIR /app
COPY pom.xml 
COPY src ./src
RUN mvn clean package -DskipTests
FROM openjdk:11-jre-slim
ENV SPRING_PROFILES_ACTIVE=prod
ENV JAVA_OPTS=""
WORKDIR /app
COPY --from=build /app/target/your-app.jar app.jar
EXPOSE 8080
VOLUME /app/config
VOLUME /app/logs
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
```        
3.  **Build the Docker Image**:
    
    -   Open a terminal in your project directory and build the Docker image:
        `docker build -t your-app:latest .` 
    -   Check if the image is created successfully:
        `docker images` 
        
4.  **Test the Docker Image Locally** (Optional):
    -   Run the Docker container locally to ensure it works:
        `docker run -p 8080:8080 your-app:latest` 
    -   Open your browser or use `curl` to verify the app at `http://localhost:8080`.

### **Step 2: Set Up the EC2 Instance**

1.  **Log in to AWS Console**:
    
    -   Go to the [AWS Management Console](https://aws.amazon.com/console/).
    -   Navigate to **EC2 Dashboard**.
2.  **Launch an EC2 Instance**:
    
    -   Click **Launch Instance**.
    -   Choose an **Amazon Machine Image (AMI)**, preferably:
        -   **Amazon Linux 2**.
        -   **Ubuntu** (for Debian-based setup).
    -   Choose an **Instance Type** (e.g., `t2.micro` for testing).
    -   **Configure Instance Details** (default settings are usually fine).
    -   **Add Storage** (default settings are sufficient unless additional storage is needed).
    -   **Configure Security Group**:
        -   Open **Port 22** for SSH access.
        -   Open **Port 8080** (or your desired application port) for HTTP access.
    -   Launch the instance and choose a **key pair** for SSH access.
3.  **Connect to the EC2 Instance**:
    -   Open a terminal and SSH into the EC2 instance using:
        `ssh -i "your-key.pem" ec2-user@your-public-ip` 
    -   Replace `"your-key.pem"` with the name of your key file and `your-public-ip` with the EC2 instance's public IP.
        

### **Step 3: Install Docker on the EC2 Instance**

1.  **Update the System**:
    `sudo yum update -y  # For Amazon Linux
    sudo apt update -y  # For Ubuntu` 
2.  **Install Docker**:
    -   For **Amazon Linux**:
        `sudo amazon-linux-extras install docker -y`   
    -   For **Ubuntu**:
        `sudo apt install docker.io -y` 
3.  **Start and Enable Docker**:
    `sudo systemctl start docker
    sudo systemctl enable docker` 
4.  **Add the EC2 user to the Docker group** (optional, allows running Docker without `sudo`):
    `sudo usermod -aG docker ec2-user` 
    -   **Logout and re-login** for the changes to take effect.
5.  **Verify Docker Installation**:
    `docker --version
    docker run hello-world`
    
### **Step 4: Transfer the Docker Image to the EC2 Instance**

1.  **Option 1: Use Docker Hub** (Recommended for easier deployments):
    -   **Create a Docker Hub account** if you don't have one.
    -   **Tag the Docker image**:
        `docker tag your-app:latest your-dockerhub-username/your-app:latest` 
    -   **Log in to Docker Hub**:
        `docker login` 
    -   **Push the Docker image** to Docker Hub:
        `docker push your-dockerhub-username/your-app:latest` 
    -   On the EC2 instance, **pull the Docker image**:
        `docker pull your-dockerhub-username/your-app:latest` 
        
2.  **Option 2: SCP (Secure Copy) the Image File Directly**:
    -   Save the Docker image to a `.tar` file on your local machine:
        `docker save -o your-app.tar your-app:latest` 
    -   Transfer the `.tar` file to EC2 using `scp`:
        `scp -i "your-key.pem" your-app.tar ec2-user@your-public-ip:/home/ec2-user` 
    -   On the EC2 instance, **load the image**:
        `docker load -i your-app.tar` 
        
### **Step 5: Run the Docker Container on EC2*
1.  **Run the Docker Container**:
    `docker run -d -p 8080:8080 your-dockerhub-username/your-app:latest` 
    -   Use the `-d` flag to run the container in detached mode.
    -   Expose the application on **port 8080**.
2.  **Verify the Container is Running**:
    `docker ps` 
3.  **Access the Application**:
    -   Navigate to vbnet
        `http://your-public-ip:8080` 

### **Step 6: Manage the Docker Container**
1.  **View Container Logs**:
    `docker logs <container-id>` 
2.  **Stop the Container**:
    `docker stop <container-id>` 
3.  **Restart the Container**:
    `docker start <container-id>` 
4.  **Remove the Container**:
    `docker rm <container-id>` 
    
### **Step 7: Optional Enhancements (Load Balancer, HTTPS, Auto-restart)**

1.  **Expose the Application via a Load Balancer** (Optional):
    -   Use **AWS Elastic Load Balancer (ELB)** for better availability and load distribution.
    -   Configure the **Load Balancer** to forward traffic from **port 80** to your application port.
2.  **Enable HTTPS**:
    -   Use **AWS Certificate Manager** for SSL certificates.
    -   Configure the Load Balancer to handle SSL termination.
3.  **Auto Restart the Docker Container** (Optional):
    -   Use the `--restart` flag to ensure the container auto-restarts if it fails:
        `docker run -d -p 8080:8080 --restart unless-stopped your-dockerhub-username/your-app:latest` 
      
### **Step 8: Monitoring and Scaling (Optional)**
1.  Use **AWS CloudWatch** for monitoring logs, performance, and resource utilization.
2.  Set up **Auto Scaling Groups** if you plan to have multiple EC2 instances to handle increased traffic.

### **Docker Commands to Manage the Spring Boot Application**
#### **1. Building the Docker Image**
`docker build -t your-app:latest .` 
-   `-t your-app:latest`: Tags the image with the name `your-app` and version `latest`.
-   `.`: Indicates the Dockerfile's location (current directory).

#### **2. Running the Docker Container**
`docker run -d -p 8080:8080 --name your-app-container -v /local/config:/app/config -v /local/logs:/app/logs your-app:latest` 
-   `-d`: Run the container in detached mode (in the background).
-   `-p 8080:8080`: Maps port 8080 on the host to port 8080 in the container.
-   `--name your-app-container`: Assigns a name to the container.
-   `-v /local/config:/app/config`: Mounts the local directory `/local/config` to the container’s `/app/config`.
-   `-v /local/logs:/app/logs`: Mounts the local directory `/local/logs` to the container’s `/app/logs`.

#### **3. View Container Logs**
`docker logs your-app-container` 

#### **4. Stop and Remove the Container**
-   **Stop** the container:
    `docker stop your-app-container` 
-   **Remove** the container:
    `docker rm your-app-container` 

#### **5. Inspect the Running Container**
`docker inspect your-app-container` 

#### **6. Pass Environment Variables at Runtime**
`docker run -d -p 8080:8080 --name your-app-container -e SPRING_PROFILES_ACTIVE=dev your-app:latest` 
-   `-e SPRING_PROFILES_ACTIVE=dev`: Passes the environment variable to the container at runtime.

#### **7. Cleanup Unused Images and Containers**
Remove unused containers `docker container prune`

Remove unused images `docker image prune`

If your Spring Boot application relies on other services (like a database), you can use Docker Compose to manage multiple containers. Here’s an example docker-compose.yml:
```
version: '3'
services:
  app:
    image: your-app:latest
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: prod
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: yourdb
    ports:
      - "5432:5432"

```

`docker-compose up -d`
`docker-compose down`



#### When choosing between Docker Swarm and Kubernetes, you can consider factors like:

**Project complexity:** Kubernetes is better for complex applications with high demand, while Docker Swarm is better for simpler applications    
**Ease of use:** Docker Swarm is designed to be easy to use and set up, while Kubernetes has a steeper learning curve
**Scalability:** Both tools can scale infrastructure up or down, but Kubernetes has automated scaling features
**Load balancing:**  Docker Swarm automatically balances load, while Kubernetes requires manual configuration
**Ecosystem:** Kubernetes has a large ecosystem with extensive support, tools, and integrations
**Security**: Kubernetes has advanced security features and configurations
**Team expertise:**  Kubernetes requires significant resources for setup and management, so it's better for teams with expertise or a willingness to learn
