[![Build Status](https://travis-ci.org/Bo-Yuan-Huang/Docker-Ubuntu-Dev.svg?branch=master)](https://travis-ci.org/Bo-Yuan-Huang/Docker-Ubuntu-Dev)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/4f015e7197484cef87c6b2718440dd11)](https://app.codacy.com/app/Bo-Yuan-Huang/Docker-VM?utm_source=github.com&utm_medium=referral&utm_content=Bo-Yuan-Huang/Docker-VM&utm_campaign=Badge_Grade_Dashboard)

## About

This repo demonstrates the use of Docker to efficiently create, run, and deploy a Linux (Ubuntu Bionic) image with several basic build packages.
It also provides scripts to build customized Docker image for personal configuration, e.g., vim, bash, git, etc. 

### Get the Ubuntu Bionic image with development packages installed

A pre-built image with packages in [setup.sh](base/setup.sh) can be fetched from Docker host
``` bash
docker pull byhuang/ubuntu-dev:base
```

To create a customized image, modify ``base/setup.sh`` and build 
``` bash
cd base
docker build -t my-image .
```

### Unpack the packages for remote user log-in

By default, the Ubuntu Docker image does not have packages needed for user log-in. 
You can get a pre-built image with remote access usibility enabled 

``` bash
docker pull byhuang/ubuntu-dev:full
```

To create a customized image with remote access, after following the previous step, modify the **FROM** entry in ``full/Dockerfile`` to my-image.

``` bash
cd full
docker build -t my-image-full .
```

### Initiate a container

Now you can run a container based on the newly fetched/created image.

``` bash
docker run -it my-image-full
```

To create a volume (shared storage) and mount to the container.

``` bash
docker volume build my-data
docker run -it \                                # interactive pseudo TTY
           -v my-data:/path/to/dir my-data \    # mount the volume (shared storage)
           --name cont-name \                   # specify container name
           --cap-add=SYS_PTRACE \               # enable debugger
           --security-opt seccomp=unconfined \  # enable debugger
           my-image-full                        # image
```

You can leave the container running in background by pressing the hot keys

``` bash
<C-p> <C-q>
```

### Personal use example

An example of automating personalization can be found in [user](user).
It shows how to create an user account (with sudo) and set up personal preferred environment.
To install customized bash prompt
``` bash
source ~/.bash_it/install.sh
```

### Notes
If you have duplicated hosts (IP being used before)
``` bash
ssh-keygen -R <host>                        # if duplicated hosts
```

If you want to reconfigure your time zone
``` bash
sudo dpkg-reconfigure tzdata                # reconfigure time zone if desired
```
