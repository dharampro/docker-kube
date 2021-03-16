# docker and kubernetes commands

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
> `docker push [OPTIONS] NAME[:TAG]`

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
