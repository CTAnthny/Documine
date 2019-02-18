# Docker

## <u>101</u>

### Overview

> Docker relies on using the host OSLinux kernel (since almost all the versions of Linux use the standard kernel models) OS it was built upon, such as Red Hat, CentOS, and Ubuntu. For this reason, you can have almost any Linux OS as your host operating system and be able to layer other Linux-based operating systems on top of the host.

> Docker images are built without the largest piece: the kernel or the operating system. This makes them incredibly small, compact, and easy to ship. <a name="fn-1"><sup>[1]</sup></a>

### FAQ:

* What is the difference between Docker and a VM (Virtual Machine)?
  > The VM is a *hardware abstraction*: it takes physical CPUs and RAM from a host, and divides and shares it across several smaller virtual machines. There is an OS and application running inside the VM, but the virtualization software usually has no real knowledge of that.
  
  > A container is an *application abstraction*: the focus is really on the OS and the application, and not so much the hardware abstraction. Many customers actually use both VMs and containers today in their environments and, in fact, may run containers inside of VMs.<a name="fn-2"><sup>[2]</sup></a>

* What are images?
  > The file system and application configuration that is used in creating a container.<a name="fn-2"><sup>[2]</sup></a>

* What are containers?
  > Running instances of Docker images — containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers, and runs as an isolated process in user space on the host OS.<a name="fn-2"><sup>[2]</sup></a>

* What are layers?
  > A Docker image is built up from a series of layers. Each layer represents an instruction in the image’s Dockerfile. Each layer except the last one is read-only.<a name="fn-3"><sup>[3]</sup></a>

* What are volumes?
  > A special Docker container layer that allows data to persist and be shared separately from the container itself. Think of volumes as a way to abstract and manage your persistent data separately from the application itself.<a name="fn-3"><sup>[3]</sup></a>

* What is a Dockerfile?
  > A text file that contains all the commands, in order, needed to build a given image. The Dockerfile reference page lists the various commands and format details for Dockerfiles.<a name="fn-3"><sup>[3]</sup></a>

* What is the *Docker daemon*?

  > The background service running on the host that manages building, running and distributing Docker containers.<a name="fn-2"><sup>[2]</sup></a>

* What is the *Docker client*?

  > The command line tool that allows the user to interact with the Docker daemon.<a name="fn-2"><sup>[2]</sup></a>

* What is the *Docker store*?
  
  > The store is, among other things, a registry of Docker images. You can think of the registry as a directory of all available Docker images.<a name="fn-2"><sup>[2]</sup></a>

* Why are there so many container instances listed from running a single image?
  > This is a critical security concept in the world of Docker containers! *Isolation*. Even though each docker container run command used the same image, each execution was a separate, isolated container. Each container has a separate filesystem and runs in a different namespace; by default a container has no way of interacting with other containers, even those from the same image.<a name="fn-2"><sup>[2]</sup></a>

### Commands

* View all images on your system: <br>`docker image ls`

* View all container instances: <br>`docker container ls -a`

* `docker container run` <br> finds the image, creates the container and then runs the following command in that container

* Start a specific container instance: <br>`docker container start <container ID>`

* Run a command in a running container: <br>`docker container exec`
(example) `docker container <container ID> ls`

* Run the container in an interactive terminal: <br>`run -it`
(example) `docker container run -it alpine /bin/sh`

* Find out more about an image: <br>`docker image inspect`

* Filter for specific details: <br>`docker image inspect --format "" <image>`
(example) `docker image inspect --format "{{ json .RootFS.Layers }}" alpine`

* Download an image: <br>`docker image pull`

* View image layers: <br>`docker image history <image ID>`

### Image Creation<a name="fn-3"><sup>[3]</sup></a>

#### By Commit

* To inspect custom changes: <br>`docker container diff <container ID>`

* Create a local image by commit: <br>`docker container commit <container ID>`

* Add a tag: : <br>`docker image tag <image ID> <name>`

* Push to the Docker Store

#### By Dockerfile

> With a Dockerfile we are supplying the instructions for building the image, rather than just the raw binary files.

* Specify a base image to pull <u>FROM</u> (ex. alpine image).

* <u>RUN</u> commands (ex: `apk update and apk add`) inside that container.

* <u>COPY</u> files from our working directory in to the container.

* Specify the <u>WORKDIR</u> - the directory the container should use when it starts up.

* Specify a command (<u>CMD</u>) to run when the container starts.

(example):
```yaml
FROM alpine
RUN apk update && apk add nodejs
COPY . /app
WORKDIR /app
CMD ["node","index.js"]
```

* Build image from dockerfile: <br>`docker image build -t <name> <path>`
(example) `docker image build -t hello:v0.1 .`

* Run the container: <br>`docker container run <name>`

### Layers

> Images themselves are actually built in layers, and each layer is immutable. As an image is created and successive layers are added, the new layers keep track of the changes from the layer below. When you start the container running there is an additional layer used to keep track of any changes that occur as the application runs. This design principle is important for both security and data management. If someone mistakenly or maliciously changes something in a running container, you can very easily revert back to its original state because the base layers cannot be changed. Or you can simply start a new container instance which will start fresh from your pristine image. And applications that create and store data (databases, for example) can store their data in a special kind of Docker object called a volume, so that data can persist and be shared with other containers.<a name="fn-3"><sup>[3]</sup></a>

## <u>Learning Resources</u>

### Free

1: [Getting Started with Docker (Udemy)](https://www.udemy.com/docker-quick-start/)

2: [Play wih Docker Classroom](https://training.play-with-docker.com/)

## <u>References</u>

[1](#fn-1): [Mastering Docker \(Packt\)](https://www.packtpub.com/virtualization-and-cloud/mastering-docker-third-edition)

[2](#fn-2): [Play with Docker (s1-containers)](https://training.play-with-docker.com/ops-s1-hello/)

[3](#fn-3): [Play with Docker (s1-images)](https://training.play-with-docker.com/ops-s1-images/)
