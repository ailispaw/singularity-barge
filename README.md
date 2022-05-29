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
  -c, --config string   specify a configuration file (for root or
                        unprivileged installation only) (default
                        "/etc/singularity/singularity.conf")
  -d, --debug           print debugging information (highest verbosity)
  -h, --help            help for singularity
      --nocolor         print without color output (default False)
  -q, --quiet           suppress normal output
  -s, --silent          only print errors
  -v, --verbose         print additional information
      --version         version for singularity

Available Commands:
  build       Build a Singularity image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  completion  Generate the autocompletion script for the specified shell
  config      Manage various singularity configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  help        Help about any command
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

Examples:
  $ singularity help <command> [<subcommand>]
  $ singularity help build
  $ singularity help instance start


For additional help or support, please visit https://www.sylabs.io/docs/
[bargee@barge ~]$ 
```


## Interact with Images

https://sylabs.io/guides/3.3/user-guide/quick_start.html#interact-with-images

```
[bargee@barge ~]$ singularity pull library://sylabsed/examples/lolcow
INFO:    Downloading library image
 79.91 MiB / 79.91 MiB [=======================================================================================] 100.00% 13.96 MiB/s 5s
WARNING: unable to verify container: lolcow_latest.sif
WARNING: Skipping container verification
[bargee@barge ~]$ singularity shell lolcow_latest.sif
Singularity lolcow_latest.sif:~> whoami
bargee
Singularity lolcow_latest.sif:~> id
uid=1000(bargee) gid=1000(bargees) groups=1000(bargees),1001(docker)
Singularity lolcow_latest.sif:~> exit
exit
[bargee@barge ~]$ singularity exec lolcow_latest.sif cowsay moo
 _____
< moo >
 -----
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
[bargee@barge ~]$ singularity run lolcow_latest.sif
 ________________________________________
< Your love life will be... interesting. >
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
[bargee@barge ~]$ ./lolcow_latest.sif
 ________________________________________
/ You will always have good luck in your \
\ personal affairs.                      /
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
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
