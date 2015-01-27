# vagrant-devbox

## About

It's my development virtual machine in using on *Windows*. It has bidirectional code sync via [unison](http://www.cis.upenn.edu/~bcpierce/unison/) and comes with [docker](https://www.docker.com/) preinstalled. Also uses [docker-compose](https://github.com/docker/fig/releases/tag/1.1.0-rc1) for docker containers orchestrating.

Also it has some docker images pulled already: [ubuntu:latest](https://registry.hub.docker.com/_/ubuntu/), [colch/devbox:master](https://registry.hub.docker.com/u/colch/devbox/), [postgres:9.4.0](https://registry.hub.docker.com/_/postgres/)

This box is [hosted on Atlas](https://atlas.hashicorp.com/ColCh/boxes/vagrant-devbox).

## Requirements

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - Free virtualization software 
* [Vagrant](https://www.vagrantup.com) - Tool for working with VirtualBox images 

## What's in box

* docker
* docker-compose
* encoding set to CP-1251
* htop
* unison profile (*unison-config.prf* file)
* pulled docker images: [ubuntu:latest](https://registry.hub.docker.com/_/ubuntu/), [colch/devbox:master](https://registry.hub.docker.com/u/colch/devbox/), [postgres:9.4.0](https://registry.hub.docker.com/_/postgres/)
* profile.d script for environment

Current dir mounts to `/vagrant.sync`

__profile.d script__ executes `/vagrant.sync/.env-dev` script, if it exists.

## Usage

Download box: 
```
vagrant box add ColCh/vagrant-devbox
```
Init *Vagrantfile*:
```
vagrant init ColCh/vagrant-devbox
```
Launch it:
```
vagrant up --provider virtualbox
```
Test docker inside of VM:
```
vagrant ssh -c "docker run hello-world"
```
Should output "Hello World"

For code sync, open second tab and ssh into machine:
```
vagrant ssh
```
And launch *unison*:
```
unison
```
This will launch *unison* with default (*~/.unison/default.prf*) config.

## Build

After some changes, remove VM:
```
vagrant destroy --force
```
Launch it:
```
vagrant up --provider virtualbox 
```
Package currently running VM into box:
```
vagrant package --vagrantfile Box_vagrantfile
```
File *package.box* will appear in directory.
Add it:
```
vagrant box add vagrant-devbox package.box
```
And use:
```
vagrant init vagrant-devbox
```
