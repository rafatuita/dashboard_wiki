This document assumes you have a Linux machine (or VM), and that you have a brand new Ubuntu Linux environment setup, but does not assume familiarity with Linux. If you don't have a Linux environment and you're using Windows, you may want to read instructions on how to setup a Linux VM on Windows first.

Before you begin please make sure you can connect to your Linux machine and login. Command line instructions for Linux will be shown starting with `$`; you should only type the text following the `$`.

This has been tested on:
* Ubuntu 17.04
* Ubuntu 16.04
* Ubuntu 14.04

## Initial System Setup

First update and upgrade your system:
```shell
$ sudo apt-get update
$ sudo apt-get upgrade
```

Install and verify software that we'll need later on:
```shell
$ sudo apt-get install curl git make g++
```

if you are using fedora (install npm3):
```shell
sudo dnf install -y fedora-repos-rawhide
sudo dnf install -y --enablerepo rawhide nodejs libuv --best --allowerasing
```

### Check

Use the following command to check which kernel version of your system:
```shell
$ uname -r
```
> You should get `3.13.0-129-generic` or something similar depending on what the current version is.

Use this in the terminal to show the details about the installed Ubuntu "version":
```shell
$ lsb_release -a
```

^ You should get a response like:
```log
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.5 LTS
Release:	14.04
Codename:	trusty
```

Use the following command to check `git` and `curl` version:
```shell
$ curl --version
$ git --version
```

These instructions were last tested with curl `7.35.0+`, and git `1.9.1+`.

## Get Vagrant on Linux

To install Vagrant using `apt-get`:
```shell
$ sudo apt-get install vagrant
$ vagrant --version
$ export KUBERNETES_PROVIDER=vagrant
$ echo "export KUBERNETES_PROVIDER=vagrant" >> ~/.profile
```

These instructions are using vagrant version `1.4.3+`.

## Install Docker

To install Docker please follow instructions from: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu.

### Configure Docker for your user

The example below uses "username" as a placeholder. Please substitute with the user you are logged in as, which can be seen by using `$ id`.
If you are running Linux in a VM using Vagrant, your username will be "vagrant".

```shell
$ sudo groupadd docker
$ sudo usermod -aG docker username
$ su - $USER
```

#### Check

To run Docker without `sudo` command:
```shell
$ docker run hello-world
```

You should get the same message as above, that includes: `This message shows that your installation appears to be working correctly`.

For an additional check you can run these commands:

`$ status docker` --> should say  "docker start/running, process [some number]"

`$ docker ps` --> should show a table of information (or at least headers)


## Install Go

The instructions below are for install a specific version of Go (1.8.3 for linux amd64). If you want the latest Go version or have a different system, then you can get the latest download URL from https://golang.org/dl/.

```shell
$ wget https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.8.3.linux-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin
$ echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
```

### Check

To get Go version:
```shell
$ go version
$ echo $PATH
```

The Go version should return something like `go version go1.8.3 linux/amd64`.

## Install Node and NPM

In order to install these dependencies we will use [Node Version Manager](https://github.com/creationix/nvm) tool.

```shell
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
$ command -v nvm
```

In case last command returns `nvm: command not found`, close and reopen your terminal, and then execute:

```shell
$ nvm install 7.10
$ nvm use 7.10
```

### Check

```shell
$ node -v
$ npm -v
```

The last time these instructions were updated, this returned `v7.10.1` and `4.2.0` respectively, but later versions will probably also work.

## Install Java runtime

To install OpenJDK 7, execute the following command:
```shell
$ sudo apt-get install openjdk-7-jre
```
> Use `Ubuntu 16.04` need to change `openjdk-7-jre`to` openjdk-8-jre`.

if you are fedora:
```shell
$ sudo dnf install java-1.8.0-openjdk
```

### Check

To get Java version:
```shell
$ java -version
```

Should return `java version "1.7.0_151"`.

## Install Gulp

To install Gulp using npm:
```shell
$ sudo npm install --global gulp-cli
$ sudo npm install --global gulp
```

### Check

To get Gulp version:
```shell
$ gulp -v
```

Should return `CLI version 3.9.1` and `Local version 3.9.1`.

## Get Kubernetes

Download the command line tool _kubectl_.

```shell
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl   
$ chmod +x ./kubectl   
$ sudo mv ./kubectl /usr/local/bin/kubectl   
```

Clone the Dashboard and Kubernetes code from the GitHub repos. *This could take a while.*

```shell
$ git clone https://github.com/kubernetes/dashboard.git
$ git clone https://github.com/kubernetes/kubernetes.git
```

## Install Other Dashboard Dependencies Automatically with NPM

To install the dependencies:
```shell
$ cd ~/dashboard
$ npm i
```

This will install all the dependencies that are in the `package.json` file in the dashboard repo. *This could take a while.*

## Run the Kubernetes Cluster

Run the script included with the dashboard that checks out the latest Kubernetes and runs it in a Docker container.

```shell
$ cd ~/dashboard   

# Start cluster at first, because the dashboard need apiserver-host for requesting.
$ gulp local-up-cluster --heapsterServerHost 'http://localhost:8082'

# Run script to build docker image with name "kubernetes-dashboard-build-image ", and then start a container.
# The parameter "serve" is necessary, otherwise, there will be a gulp task error.
$ sudo build/run-gulp-in-docker.sh serve
```

If you need append ENV variables to container, you should edit the Dockerfile.

```Dockerfile
ENV http_proxy="http://username:passowrd@10.0.58.88:8080/"
ENV https_proxy="http://username:password@10.0.58.88:8080/"
```

If you need to stop the cluster you can run `$ docker kill $(docker ps -aq)`,
and the dashboard container is stopped also.

### Check

Open up another terminal to your machine, and try to access the dashboard.

```shell
$ curl http://localhost:9090
```

This should return the HTML for the dashboard.

### Continue

Now you may [continue with the Getting Started guide](getting-started.md) to learn more about developing with the Kubernetes Dashboard.

# Troubleshooting

## Docker

If you're having trouble with the `gulp local-up-cluster` step, you may want to investigate the docker containers.

* `docker ps -a` lists all docker containers
* `docker inspect name_of_container | grep "Error"` will look through the details of a docker container and display any errors.

If you have an error like "linux mounts: Path /var/lib/kubelet is mounted on / but it is not a shared mount." you should try `sudo mount --bind /var/lib/kubelet /var/lib/kubelet` followed by `sudo mount --make-shared /var/lib/kubelet`. ([source](https://github.com/kubernetes/kubernetes/issues/4869#issuecomment-193640483))

## Go

If you run into an error like "Go is not on the path.", you may need to re-run `export PATH=$PATH:/usr/local/go/bin`

## Helpful Linux Tips

* `env` will show your environment variables. One common error is not having every directory needed in your PATH.

Using *vim* to edit files may be helpful for beginners.

* `sudo apt-get install vim` will get *vim*
* `sudo vim /path/to/folder/filename` will open the file you want to edit.
* <kbd>i</kbd> = insert
* <kbd>Esc</kbd> = stops inserting
* `:x` = exits and saves