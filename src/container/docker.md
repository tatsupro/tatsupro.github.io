# Docker

> ⚠️ Trang còn sơ sài, cần cập nhật sang tiếng Việt

- [Docker Quick Start](https://learn.acloud.guru/course/da6947b1-0901-4f60-a045-c15ec895a3d9/dashboard)
- [Introduction to Containers and Docker](https://learn.acloud.guru/course/introduction-to-containers-and-docker/dashboard)

---

🐳 Docker là một công cụ dùng để đóng gói phần mềm dưới dạng container, dễ dàng chạy linh hoạt trên các môi trường khác nhau.

**Liệu container về bản chất khác gì máy ảo không?**
Không, cả 2 đều tạo ra một môi trường tách biệt với môi trường host. Chỉ khác là một cái chỉ không ảo hóa phần cứng và một cái có, hiệu năng sẽ cải thiện hơn chút. Với sysadmin thì sẽ không thấy khác biệt nhiều, nhưng với dev thì hiệu quả rõ rệt hơn.

**Kiến thức có thể liên quan: Stateful và Stateless**
State ở đây chỉ trạng thái của thông tin mà ứng dụng xử lý.
Về cơ bản nói đơn giản và dễ hiểu thì Stateless là những ứng dụng chỉ đơn giản nhận request, thông tin của client rồi trả về mà bản thân nó không lưu lại bất kì dữ liệu gì liên quan tới request từ client đó; còn Stateful sẽ lưu lại (và thường là trong database).
Dựa vào usecase của ứng dụng muốn thiết kế mà lựa chọn 1 trong 2 tính chất. Và tùy theo hai tính chất này, ứng dụng có thể được triển khai trên container khác nhau.

# Introduction

## 🔍 Problems

Imagine you're build an with COBOL on weird flavor Linux distro like Puppy, you want to share this application with your friend but he has an different system.
So how do we replicate the environment of our software needs on any machine❓

![](/assets/docker-1.png)

🖥️ One way to package an app is a virtual machine where the hardware is simulated then installed with the required os and dependencies. This allows us to run multiple apps on the same infrastructure, however because each VM is running its own operating system they
tend to be bulky and slow.

🐳 A Docker container is similar to a VM in concept, but with one key difference: instead of virtualizing hardware, containers virtualize only the OS. In other words, all applications or containers are run by a single kernel, resulting in improved speed and efficiency.

![](/assets/docker-2.png)

## 🏛️ Fundamental elements

There are three fundamental elements in the universe of Docker: Dockerfile, Image and Container.
The 🗃️ **Dockerfile** is like DNA, it's just code that tells Docker how to build an 🖼️ **Image** which itself is a snapshot of your software along with all of its dependencies down to the operating system level the image is immutable and it can be used to spin up multiple 🪣 **Containers** which is your actual software running in the real world.

# ❗️🚧 Underlying Technologies

> 🚧💬 This section is pretty sloppy. It's got too many words saying the same thing and abstract stuff that's hard to understand. You should probably fix it later.

Understanding the core technologies that power Docker will provide you with a deeper insight into how Docker works and will help you use the platform more effectively.
**tl;dr**: Containers are just lightweight isolated process running on Linux.

![](/assets/docker-3.png)

ℹ️ _NOTE: In the version 0.9, Docker replaced LXC with its own component, libcontainer, which was written in the [Go](<https://en.wikipedia.org/wiki/Go_(programming*language)> "Go (programming language)") programming language.*

## Linux Containers (LXC)

Linux Containers (LXC) serve as the foundation for Docker. LXC is a lightweight virtualization solution that allows multiple isolated Linux systems to run on a single host without the need for a full-fledged hypervisor. LXC effectively isolates applications and their dependencies in a secure and optimized manner.

## Control Groups (cgroups)

**cgroups** or **control groups** is a Linux kernel feature that allows you to allocate and manage resources, such as CPU, memory, network bandwidth, and I/O, among groups of processes running on a system. It plays a crucial role in providing resource isolation and limiting the resources that a running container can use.
Docker utilizes cgroups to enforce resource constraints on containers, allowing them to have a consistent and predictable behavior. Below are some of the key features and benefits of cgroups in the context of Docker containers:

### Resource Isolation

cgroups helps to confine each container to a specific set of resources, ensuring fair sharing of system resources among multiple containers. This enables better isolation between different containers, so that a misbehaving container does not consume all available resources, thereby negatively affecting other containers.

### Limiting Resources

With cgroups, you can set limits on various system resources used by a container, such as CPU, memory, and I/O. This helps to prevent a single container from consuming excessive resources and causing performance issues for other containers or the host system.

### Prioritizing Containers

By allocating different shares of resources, cgroups allows you to give preference or priority to certain containers. This can be useful in scenarios where some containers are more critical than others, or during high resource contention situations.

### Monitoring

cgroups also offers mechanisms for monitoring the resource usage of individual containers, which helps to gain insights into container performance and identify potential resource bottlenecks.
Overall, cgroups is an essential underlying technology in Docker. By leveraging cgroups, Docker provides a robust and efficient container runtime environment, ensuring the containers have the required resources while maintaining good overall system performance.

## Union File Systems (UnionFS)

Union filesystems, also known as UnionFS, play a crucial role in the overall functioning of Docker. It’s a unique type of filesystem that creates a virtual, layered file structure by overlaying multiple directories. Instead of modifying the original file system or merging directories, UnionFS enables the simultaneous mounting of multiple directories on a single mount point while keeping their contents separate. This feature is especially beneficial in the context of Docker, as it allows us to manage and optimize storage performance by minimizing duplication and reducing the container image size.
These are some of the essential features of union filesystems:

- **Layered Structure**: UnionFS builds a layered structure consisting of multiple read-only layers and a top writable layer. This structure enables efficient handling of changes by only updating the writable layer, while the read-only layers preserve the original data.
- **Copy-on-Write**: The copy-on-write (COW) mechanism is an indispensable feature of UnionFS. If a container makes changes to an existing file, the system creates a copy of the file in the writable layer, leaving the original file in the read-only layer untouched. This process restricts modification to the topmost layer, ensuring a fast and resource-efficient operation.
- **Resource Sharing**: Union filesystems allow multiple containers to share common base layers while running separately. This feature prevents resource duplication and saves significant storage space.
- **Fast Container Initialization**: Union filesystems make it possible to create new containers instantly by merely creating a new writable layer on existing read-only layers. This quick initialization reduces the overhead of duplicated file operations, ultimately improving performance.

## Popular Union Filesystems in Docker

Docker supports multiple union filesystems that facilitate building and managing containers. Some of the popular options include:

- [**AUFS (Advanced Multi-Layered Unification Filesystem)**](http://aufs.sourceforge.net/): AUFS is widely used as a Docker storage driver, enabling efficient management of multiple layers.
- [**OverlayFS (Overlay Filesystem)**](https://www.kernel.org/doc/html/latest/filesystems/overlayfs.html): OverlayFS is another union filesystem supported by Docker. It uses a simplified approach compared to AUFS to create and manage overlayed directories.
- [**Btrfs (B-Tree Filesystem)**](https://btrfs.wiki.kernel.org/index.php/Main_Page): Btrfs, a modern file system, offers compatibility with union filesystems in addition to advanced storage features like snapshots and checksumming.
- [**ZFS (Z File System)**](https://zfsonlinux.org/): ZFS is a high-capacity and robust storage platform that provides union filesystem features along with data protection, compression, and deduplication.

## Namespaces

Namespaces are one of the core technologies that Docker uses to provide isolation between containers. In this section, we’ll briefly discuss what namespaces are and how they work.

### What are Namespaces?

In the Linux kernel, namespaces are a feature that allows the isolation of various system resources, making it possible for a process and its children to have a view of a subset of the system that is separate from other processes. Namespaces help to create an abstraction layer to keep containerized processes separate from one another and from the host system.
There are several types of namespaces in Linux, including:

- **PID (Process IDs)**: Isolates the process ID number space, which means that processes within a container only see their own processes, not those on the host or in other containers.
- **Network (NET)**: Provides each container with a separate view of the network stack, including its own network interfaces, routing tables, and firewall rules.
- **Mount (MNT)**: Isolates the file system mount points in such a way that each container has its own root file system, and mounted resources appear only within that container.
- **UTS (UNIX Time Sharing System)**: Allows each container to have its own hostname and domain name, separate from other containers and the host system.
- **User (USER)**: Maps user and group identifiers between the container and the host, so different permissions can be set for resources within the container.
- **IPC (Inter-Process Communication)**: Allows or restricts the communication between processes in different containers.

### How Docker uses Namespaces

Docker uses namespaces to create isolated environments for containers. When a container is started, Docker creates a new set of namespaces for that container. These namespaces only apply within the container, so any processes running inside the container have access to a subset of system resources that are isolated from other containers as well as the host system.
By leveraging namespaces, Docker ensures that containers are truly portable and can run on any system without conflicts or interference from other processes or containers running on the same host.
In summary, namespaces provide a level of resource isolation that enables running multiple containers with separate system resources within the same host, without them interfering with each other. This is a critical feature that forms the backbone of Docker’s container technology.

# Installation / Setup

> ℹ️ Lack of information and clear instructions. Should probably fix it later.

Docker provides a desktop application called **Docker Desktop** that simplifies the installation and setup process. There is also another option to install using the **Docker Engine**.

- [Docker Desktop website](https://www.docker.com/products/docker-desktop).
- [Docker Engine](https://docs.docker.com/engine/install/).

# Docker Basics

# Data Persistence

# Using 3rd Party Container Images

# Building Container Images

# Container Registries

# Running Containers

# Container Security

# Docker CLI

# Developer Experience

# Deploying Containers

# ℹ️ Reference

- https://www.bretfisher.com/kubernetes-vs-docker/
- https://opencontainers.org/about/overview/
- https://github.com/BretFisher/udemy-docker-mastery/blob/main/intro/what-is-docker/what-is-docker.md
