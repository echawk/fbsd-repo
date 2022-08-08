# fbsd-repo

An experimental kiss repo for [FreeBSD](https://freebsd.org).

## Installation (WIP)

The installation currently requires you to hijack an already existing
FreeBSD installation. This will change in the future.

On the FreeBSD machine (and as root), install git, curl & gzip with `pkg`:

```shell
yes | pkg install git curl gzip
```

These are all of the tools necessary to be able to start building packages in this repo.

```
cd core
export KISS_PATH="$PWD"
kiss build zlib xz pigz gmake bzip2
cd $OLDPWD

# Build perl so we can build openssl & it's dependents
cd extra/perl
kiss build
cd $OLDPWD

kiss build openssl curl git kiss

# Now we can remove all traces of `pkg`
rm -Rf /var/db/pkg/*
rm -Rf /usr/local/*

# However, we now have to rebuild curl & it's dependants, since configure picked up some stuff
/etc/ssl/update-certdata.sh
kiss build curl git
```
## TODO
* Use a git module for the upstream kiss repository, so less maintenance is required.
* Generate a rootfs
* Make sure you can perform an installation via rootfs (& add docs on how to)
