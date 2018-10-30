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
Welcome to Barge 2.10.2, Docker version 1.10.3, build 20f81dd
[bargee@barge ~]$ singularity

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
  -d, --debug              print debugging information (highest verbosity)
  -h, --help               help for singularity
  -q, --quiet              suppress normal output
  -s, --silent             only print errors
  -t, --tokenfile string   path to the file holding your sylabs
                           authentication token (default
                           "/home/bargee/.singularity/sylabs-token")
  -v, --verbose            print additional information
      --version            version for singularity

Available Commands:
  build       Build a new Singularity container
  capability  Manage Linux capabilities on containers
  exec        Execute a command within container
  help        Help about any command
  inspect     Display metadata for container if available
  instance    Manage containers running in the background
  keys        Manage OpenPGP key stores
  pull        Pull a container from a URI
  push        Push a container to a Library URI
  run         Launch a runscript within container
  run-help    Display help for container if available
  search      Search the library
  shell       Run a Bourne shell within container
  sign        Attach cryptographic signatures to container
  test        Run defined tests for this particular container
  verify      Verify cryptographic signatures on container
  version     Show application version

Examples:
  $ singularity help <command>
      Additional help for any Singularity subcommand can be seen by appending
      the subcommand name to the above command.


For additional help or support, please visit https://www.sylabs.io/docs/
[bargee@barge ~]$ 
```


## Interact with Images

http://singularity.lbl.gov/quickstart#interact-with-images  
https://www.sylabs.io/guides/3.0/user-guide/quick_start.html#interact-with-images  

```
[bargee@barge ~]$ singularity pull shub://vsoch/hello-world
WARNING: Authentication token file not found : Only pulls of public images will succeed
 62.32 MiB / 62.32 MiB [=======================================================================================] 100.00% 10.24 MiB/s 6s
[bargee@barge ~]$ singularity shell hello-world_latest.sif
Singularity hello-world_latest.sif:~> ls /
bin   dev      etc   lib    media  opt   rawr.sh  run   singularity  sys  usr
boot  environment  home  lib64  mnt    proc  root     sbin  srv      tmp  var
Singularity hello-world_latest.sif:~> exit
exit
[bargee@barge ~]$ singularity exec hello-world_latest.sif ls /
bin   dev      etc   lib    media  opt   rawr.sh  run   singularity  sys  usr
boot  environment  home  lib64  mnt    proc  root     sbin  srv      tmp  var
[bargee@barge ~]$ singularity run hello-world.simg
RaawwWWWWWRRRR!!
[bargee@barge ~]$ ./hello-world_latest.sif
RaawwWWWWWRRRR!!
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
