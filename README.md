# Singularity on Barge with Vagrant

[Singularity](http://singularity.lbl.gov/) is a container platform focused on supporting "Mobility of Compute."

> Singularity enables users to have full control of their environment. Singularity containers can be used to package entire scientific workflows, software and libraries, and even data.

This repo creates an environment to use Singularity on [Barge](https://atlas.hashicorp.com/ailispaw/boxes/barge) with [Vagrant](https://www.vagrantup.com/) instantly.

## Requirements

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

## Boot up

```
$ vagrant up
```

That's it.

Now you can use `singularity` on the Barge VM.

```
$ vagrant ssh
Welcome to Barge 2.7.3, Docker version 1.10.3, build 20f81dd
[bargee@barge ~]$ singularity
USAGE: singularity [global options...] <command> [command options...] ...

GLOBAL OPTIONS:
    -d|--debug    Print debugging information
    -h|--help     Display usage summary
    -s|--silent   Only print errors
    -q|--quiet    Suppress all normal output
       --version  Show application version
    -v|--verbose  Increase verbosity +1
    -x|--sh-debug Print shell wrapper debugging information

GENERAL COMMANDS:
    help       Show additional help for a command or container
    selftest   Run some self tests for singularity install

CONTAINER USAGE COMMANDS:
    exec       Execute a command within container
    run        Launch a runscript within container
    shell      Run a Bourne shell within container
    test       Launch a testscript within container

CONTAINER MANAGEMENT COMMANDS:
    apps       List available apps within a container
    bootstrap  *Deprecated* use build instead
    build      Build a new Singularity container
    check      Perform container lint checks
    inspect    Display container's metadata
    mount      Mount a Singularity container image
    pull       Pull a Singularity/Docker container to $PWD

COMMAND GROUPS:
    image      Container image command group
    instance   Persistent instance command group


CONTAINER USAGE OPTIONS:
    see singularity help <command>

For any additional help or support visit the Singularity
website: http://singularity.lbl.gov/

[bargee@barge ~]$ 
```


## Interact with Images

http://singularity.lbl.gov/quickstart#interact-with-images

```
[bargee@barge ~]$ singularity pull --name hello-world.simg shub://vsoch/hello-world
Progress |===================================| 100.0%
Done. Container is at: /mnt/data/home/bargee/hello-world.simg
[bargee@barge ~]$ singularity shell hello-world.simg
Singularity: Invoking an interactive shell within container...

Singularity hello-world.simg:~> ls /
bin   dev    etc   lib  media  opt   rawr.sh  run   singularity  sys  usr
boot  environment  home  lib64  mnt    proc  root     sbin  srv    tmp  var
Singularity hello-world.simg:~> exit
exit
[bargee@barge ~]$ singularity exec hello-world.simg ls /
bin   dev    etc   lib  media  opt   rawr.sh  run   singularity  sys  usr
boot  environment  home  lib64  mnt    proc  root     sbin  srv    tmp  var
[bargee@barge ~]$ singularity run hello-world.simg
RaawwWWWWWRRRR!!
[bargee@barge ~]$ ./hello-world.simg
RaawwWWWWWRRRR!!
```

## Build an image with Singularity recipe file

```
[bargee@barge ~]$ sudo singularity build sl.simg /vagrant/recipes/sl.Singularity
[bargee@barge ~]$ ./sl.simg
[bargee@barge ~]$ singularity run sl.simg
[bargee@barge ~]$ singularity exec sl.simg sl
[bargee@barge ~]$ singularity shell sl.simg
Singularity: Invoking an interactive shell within container...

Singularity sl.simg:~> sl
Singularity sl.simg:~> exit
exit
```
