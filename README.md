# Docker Commands

## Docker check version
> `docker version` \
> `docker -v`

## Create context in docker
> `docker context create [options] CONTEXT` \
>  `docker context create --docker host=unix:///var/run/docker.sock --kubernetes config-file=/home/me/my-kube-config my-context`

## Set environment

### for windows client
> `$env:DOCKER_HOST="tcp://54.204.22.14:2375"`
### for linux/mac client
> `export DOCKER_HOST="tcp://54.204.22.14:2375"`\

## Docker image
### List images
> `docker images`
### Pull Image
> `docker  pull  [image]:[tag]`
> `docker  pull  oraclelinux:8.3`
### Push docker images

    To push image we need to first need to tag first

> `docker  tag  [IMAGE]:[TAG]  [REGISTRY_TAG]/[IMAGE]:[TAG]`\
> `docker push [OPTIONS] NAME[:TAG]`\
> `docker  tag  myimage:v1  dharmendrakawasthi/myimage:v1`\
> `docker push dharmendrakawasthi/myimage:v1`

### Build image fromfile
> `docker image build [OPTIONS] PATH | URL | -`
> `docker build -t svendowideit/ambassador /path/to/dockerfile`

## Docker file referance

### comments
    Comments get removed before the docker file instruction get executed

> `# comment`\
> `RUN "Hello World"`

## Docker file instructions

### FROM statement
> `FROM oraclelinux:8.3`
    
    The FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions. As such, a valid Dockerfile must start with a FROM instruction, ARG instruction can preceed fromtoser some temporary command arguments.

> `ARG  CODE_VERSION=latest`\
> `FROM base:${CODE_VERSION}`\
> `CMD  /code/run-app`

### RUN command
> `RUN ["executable", "param1", "param2"]`\
> `RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'`

### CMD commands

    To execute the commands. Only one CMD can be passed in file, if there are multiple only last one will be considered
    Three forms of CMD are listed below:

> `CMD ["executable","param1","param2"]` (exec form, this is the preferred form)\
> `CMD ["param1","param2"]`(as default parameters to ENTRYPOINT)\
> `CMD command param1 param2` (shell form)

### LABEL 

> `LABEL maintainer="SvenDowideit@home.org.au"`
> `LABEL "com.example.vendor"="ACME Incorporated"`\
> `LABEL com.example.label-with-value="foo"`\
> `LABEL version="1.0"\`
> `LABEL description="This text illustrates` \
> `that label-values can span multiple lines."`

    The LABEL instruction adds metadata to an image. A LABEL is a key-value pair. To include spaces within a LABEL value, use quotes and backslashes as you would in command-line parsing. A few usage examples:

### Expose command
> `EXPOSE 80/udp`

    The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.

### ENV statement
    set the environment variable

> `ENV FOO=/bar`

    Variable FOO can be accesed by using ${FOO}

### ADD 

> `ADD hom.txt /mydir/`
> `ADD http://somefile.com/file.txt /mydir/`

    The ADD instruction copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.

### COPY 

> `COPY test.txt relativeDir/`

    The COPY instruction copies new files or directories from <src> and adds them to the filesystem of the container at the path <dest>. Multiple <src> resources may be specified but the paths of files and directories will be interpreted as relative to the source of the context of the build.

### ENTRYPOINT

> `ENTRYPOINT ["executable", "param1", "param2"]`\
> `ENTRYPOINT command param1 param2`

    An ENTRYPOINT allows you to configure a container that will run as an executable.

### VOLUME 
> `VOLUME ["/data"]`

    The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers

### USER

> `USER <user>[:<group>]`

    The USER instruction sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile.

### WORKDIR

> `WORKDIR /path/to/workdir`

    The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.

### ARG 

> `ARG <name>[=<default value>]`

    The ARG instruction defines a variable that users can pass at build-time to the builder with the docker build command using the --build-arg <varname>=<value> flag


### Example Docker file

    Create image by base image Oracle Linux 8.3 and copy python file fromlocaland run it.

> `ARG VERSION = 8.3`\
> `FROM oraclelinux: VERSION`\
> `LABEL maintainer="dharam@linux.com"`\
> `RUN dnf install python3 -y`\
> `RUN mkdir /mycode`\
> `ADD hello.py /mycode/hello.py`\
> `CMD ["python3", "/mycode/hello.py"]`

### Removing Image 

> `docker rmi -i [IMAGE]:[TAG]`\
> e.g: `docker rmi oraclelinux:8.3`


### Save Image to local

> `docker  save -o  [FILE_NAME].tar  [IMAGE]:[TAG]`\
> e.g: `docker  save -o  mywebimg.tar  myimage:v1`

### Load local image

> `docker load -i [MYFILE].tar`\
> `docker load -i  mywebimg.tar`

### Inspect Docker image

> `docker  inspect  myimage:v1  -f  '{{.Config.Cmd}}'`

## Docker container

### Create docker container
> `docker run -itd --name [CONTAINER_NAME]  [IMAGE]:[TAG]   [PROCESS]`\
> `docker run -itd --name mycontainer  myimage:v1   ping 127.0.0.1`

### Delete Docker image

> `docker rm [container | container_id`\
> `docker rm container_name`

## Docker Networking

### list network bridges
> `docker  network ls`
### Inspect Network
> `docker  network   inspect  [NETWORK_ID] | [NETWORK_NAME]`
### create new network
> `docker create [OPTIONS] NETWORK`\
> `docker network create -d bridge my-bridge-network`
#### Complex network example
> docker network create -d overlay \
  --subnet=192.168.10.0/25 \
  --subnet=192.168.20.0/25 \
  --gateway=192.168.10.100 \
  --gateway=192.168.20.100 \
  --aux-address="my-router=192.168.10.5" --aux-address="my-switch=192.168.10.6" \
  --aux-address="my-printer=192.168.20.5" --aux-address="my-nas=192.168.20.6" \
  my-multihost-network

> <a href="https://docs.docker.com/engine/reference/commandline/network_create/">More about docker network</a>

### Docker exec to acess container
> `docker exec [OPTIONS]  CONTAINER COMMAND [ARGS...]`
#### Create a file
> `docker exec -d ubuntu_bash touch /tmp/execWorks`
#### access terminal
> `docker exec -it ubuntu_bash bash`\
> <a href="https://docs.docker.com/engine/reference/commandline/exec/"> More About docker exec</a>

### container volume

    Volumes are the preferred mechanism for persisting data generated by and used by Docker containers. While bind mounts are dependent on the directory structure and OS of the host machine, volumes are completely managed by Docker.

#### List volume
> `docker  volume  ls`
#### Create volume
> `ocker  volume  create  VOLUME_NAME`
#### Volume inspect
> `docker  volume  inspect  VOLUME`
#### delete volume
> `docker volume rm MY_VOLUME`
#### Mount volume in container
> `docker run -d --name devtest --mount source=myvol2,target=/app  IMAGE:TAG`

## Main commands
> Usage:  docker [OPTIONS] COMMAND

> Options:
> 
> 
>       --config string      Location of client config files (default "/Users/a13401651/.docker")
> 
>   -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default
>                           context set with "docker context use")
>  
>  -D, --debug              Enable debug mode
>  
>  -H, --host list          Daemon socket(s) to connect to
>  
>  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
>      
>      --tls                Use TLS; implied by --tlsverify
>      
>      --tlscacert string   Trust certs signed only by this CA (default "/Users/a13401651/.docker/ca.pem")
>      
>      --tlscert string     Path to TLS certificate file (default "/Users/a13401651/.docker/cert.pem")
>      
>      --tlskey string      Path to TLS key file (default "/Users/a13401651/.docker/key.pem")
>      
>      --tlsverify          Use TLS and verify the remote
>  
>  -v, --version            Print version information and quit
>
> Management Commands:
> 
>  builder     Manage builds
> 
>  buildx*     Docker Buildx (Docker Inc., v0.10.3)
>  
>  compose*    Docker Compose (Docker Inc., v2.15.1)
>  
>  config      Manage Docker configs
>  
>  container   Manage containers
>  
>  context     Manage contexts
>  
>  dev*        Docker Dev Environments (Docker Inc., v0.1.0)
>  
>  extension*  Manages Docker extensions (Docker Inc., v0.2.18)
>  
>  image       Manage images
>  
>  manifest    Manage Docker image manifests and manifest lists
>  
>  network     Manage networks
>  
>  node        Manage Swarm nodes
>  
>  plugin      Manage plugins
>  
>  sbom*       View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc., 0.6.0)
>  
>  scan*       Docker Scan (Docker Inc., v0.25.0)
>  
>  scout*      Command line tool for Docker Scout (Docker Inc., v0.6.0)
>  
>  secret      Manage Docker secrets
>  
>  service     Manage services
>  
>  stack       Manage Docker stacks
>  
>  swarm       Manage Swarm
>  
>  system      Manage Docker
>  
>  trust       Manage trust on Docker images
>  
>  volume      Manage volumes

> Commands:
>  
>  attach      Attach local standard input, output, and error streams to a running container
>
>  build       Build an image from a Dockerfile
>  
>  commit      Create a new image from a container's changes
>  
>  cp          Copy files/folders between a container and the local filesystem
>  
>  create      Create a new container
>  
>  diff        Inspect changes to files or directories on a container's filesystem
>  
>  events      Get real time events from the server
>  
>  exec        Run a command in a running container
>  
>  export      Export a container's filesystem as a tar archive
>  
>  history     Show the history of an image
>  
>  images      List images
>  
>  import      Import the contents from a tarball to create a filesystem image
>  
>  info        Display system-wide information
>  
>  inspect     Return low-level information on Docker objects
>  
>  kill        Kill one or more running containers
>  
>  load        Load an image from a tar archive or STDIN
>  
>  login       Log in to a Docker registry
>  
>  logout      Log out from a Docker registry
>  
>  logs        Fetch the logs of a container
>  
>  pause       Pause all processes within one or more containers
>  
>  port        List port mappings or a specific mapping for the container
>  
>  ps          List containers
>  
>  pull        Pull an image or a repository from a registry
>  
>  push        Push an image or a repository to a registry
>  
>  rename      Rename a container
>  
>  restart     Restart one or more containers
>  
>  rm          Remove one or more containers
>  
>  rmi         Remove one or more images
>  
>  run         Run a command in a new container
>  
>  save        Save one or more images to a tar archive (streamed to STDOUT by default)
>  
>  search      Search the Docker Hub for images
>  
>  start       Start one or more stopped containers
>  
>  stats       Display a live stream of container(s) resource usage statistics
>  
>  stop        Stop one or more running containers
>  
>  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
>  
>  top         Display the running processes of a container
>  
>  unpause     Unpause all processes within one or more containers
>  
>  update      Update configuration of one or more containers
>  
>  version     Show the Docker version information
>  
>  wait        Block until one or more containers stop, then print their exit codes

> Run 'docker COMMAND --help' for more information on a command.

> To get more help with docker, check out our guides at https://docs.docker.com/go/guides/



