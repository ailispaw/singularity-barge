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
Welcome to Barge 2.15.0, Docker version 1.10.3, build 662b14f
[bargee@barge ~]$ singularity
Usage:
  singularity [global options...] <command>

Available Commands:
  build       Build a Singularity image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  completion  Generate the autocompletion script for the specified shell
  config      Manage various singularity configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  oci         Manage OCI containers
  overlay     Manage an EXT3 writable overlay image
  plugin      Manage Singularity plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  remote      Manage singularity remote endpoints, keyservers and OCI/Docker registry credentials
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         Manipulate Singularity Image Format (SIF) images
  sign        Attach digital signature(s) to an image
  test        Run the user-defined tests within a container
  verify      Verify cryptographic signatures attached to an image
  version     Show the version for Singularity

Run 'singularity --help' for more detailed usage information.
[bargee@barge ~]$ 
```


## Interact with Images

http://singularity.lbl.gov/quickstart#interact-with-images  
https://www.sylabs.io/guides/3.0/user-guide/quick_start.html#interact-with-images  

```
[bargee@barge ~]$ singularity pull shub://vsoch/hello-world
INFO:    Downloading shub image
59.8MiB / 59.8MiB [======================================================================================================================] 100 % 15.1 MiB/s 0s
[bargee@barge ~]$ singularity shell hello-world_latest.sif
Singularity> ls /
bin  boot  dev	environment  etc  home	lib  lib64  media  mnt	opt  proc  rawr.sh  root  run  sbin  singularity  srv  sys  tmp  usr  var
Singularity> exit
exit
[bargee@barge ~]$ singularity exec hello-world_latest.sif ls /
bin  boot  dev	environment  etc  home	lib  lib64  media  mnt	opt  proc  rawr.sh  root  run  sbin  singularity  srv  sys  tmp  usr  var
[bargee@barge ~]$ singularity run hello-world_latest.sif
RaawwWWWWWRRRR!! Avocado!
[bargee@barge ~]$ ./hello-world_latest.sif
RaawwWWWWWRRRR!! Avocado!
```

## Build an image with Singularity recipe file

```
[bargee@barge ~]$ sudo mkdir -p /mnt/data/tmp
[bargee@barge ~]$ sudo singularity build --tmpdir=/mnt/data/tmp sl.sif /vagrant/recipes/sl.Singularity
[bargee@barge ~]$ ./sl.sif
[bargee@barge ~]$ singularity run sl.sif
[bargee@barge ~]$ singularity exec sl.sif sl
[bargee@barge ~]$ singularity shell sl.sif
Singularity sl.sif:~> sl
Singularity sl.sif:~> exit
exit
```
