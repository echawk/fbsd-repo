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

# Make sure that you are building the proper version of bsd-base
# ie. If you are on a FreeBSD 13.0 system, edit bsd-base/version to reflect this.

kiss b bsd-base
# see below for installation issues.

```

## Current error:
Currently kiss can *build* bsd-base just fine, however it cannot *install* it as of yet.
The package manager gets as far as printing the line:
```
Installing package (bsd-base\@13.0...)
```
And it is followed by failed removal attempts of the files under `/root/.cache/kiss/proc/*/`
Most if not all of the files that it is trying to remove have special permissions.
They are removable by running the following:
```
chflags -R noschg /root/.cache/kiss/proc/
chmod 644 /root/.cache/kiss/proc/
rm -rf /root/.cache/kiss/proc/
```

I think that there will need to be a light patch to kiss to help facilitate this change.

## TODO
* Use a git module for the upstream kiss repository, so less maintenance is required.
* Generate a rootfs
* Make sure you can perform an installation via rootfs (& add docs on how to)
