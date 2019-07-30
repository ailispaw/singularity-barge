# Singularity on Barge with Vagrant

[Singularity](https://github.com/sylabs/singularity) is an open source container platform designed to be simple, fast, and secure.

> Singularity is optimized for EPC and HPC workloads, allowing untrusted users to run untrusted containers in a trusted way.

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
Welcome to Barge 2.13.0, Docker version 1.10.3, build 20f81dd
[bargee@barge ~]$ singularity -h

Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  singularity [global options...]

Description:
  Singularity containers provide an application virtualization layer enabling
  mobility of compute via both application and environment portability. With
  Singularity one is capable of building a root file system that runs on any
  other Linux system where Singularity is installed.

Options:
  -d, --debug     print debugging information (highest verbosity)
  -h, --help      help for singularity
      --nocolor   print without color output (default False)
  -q, --quiet     suppress normal output
  -s, --silent    only print errors
  -v, --verbose   print additional information
      --version   version for singularity

Available Commands:
  build       Build a Singularity image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  exec        Run a command within a container
  help        Help about any command
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  oci         Manage OCI containers
  plugin      Manage singularity plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  remote      Manage singularity remote endpoints
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         siftool is a program for Singularity Image Format (SIF) file manipulation
  sign        Attach a cryptographic signature to an image
  test        Run the user-defined tests within a container
  verify      Verify cryptographic signatures attached to an image
  version     Show the version for Singularity

Examples:
  $ singularity help <command> [<subcommand>]
  $ singularity help build
  $ singularity help instance start


For additional help or support, please visit https://www.sylabs.io/docs/
[bargee@barge ~]$ 
```


## Interact with Images

http://singularity.lbl.gov/quickstart#interact-with-images  
https://sylabs.io/guides/3.3/user-guide/quick_start.html#interact-with-images

```
[bargee@barge ~]$ singularity pull shub://vsoch/hello-world
WARNING: Authentication token file not found : Only pulls of public images will succeed
 62.32 MiB / 62.32 MiB [========================================================================================] 100.00% 8.52 MiB/s 7s
[bargee@barge ~]$ singularity shell hello-world_latest.sif
Singularity hello-world_latest.sif:~> ls /
bin   dev	   etc	 lib	media  opt   rawr.sh  run   singularity  sys  usr
boot  environment  home  lib64	mnt    proc  root     sbin  srv		 tmp  var
Singularity hello-world_latest.sif:~> exit
exit
[bargee@barge ~]$ singularity exec hello-world_latest.sif ls /
bin   dev	   etc	 lib	media  opt   rawr.sh  run   singularity  sys  usr
boot  environment  home  lib64	mnt    proc  root     sbin  srv		 tmp  var
[bargee@barge ~]$ singularity run hello-world_latest.sif
RaawwWWWWWRRRR!! Avocado!
[bargee@barge ~]$ ./hello-world_latest.sif
RaawwWWWWWRRRR!! Avocado!
```

## Build an image with Singularity recipe file

```
[bargee@barge ~]$ sudo singularity build sl.sif /vagrant/recipes/sl.Singularity
[bargee@barge ~]$ ./sl.sif
[bargee@barge ~]$ singularity run sl.sif
[bargee@barge ~]$ singularity exec sl.sif sl
[bargee@barge ~]$ singularity shell sl.sif
Singularity sl.sif:~> sl
Singularity sl.sif:~> exit
exit
```
