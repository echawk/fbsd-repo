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

## TODO
* Use a git module for the upstream kiss repository, so less maintenance is required.
* Generate a rootfs
* Make sure you can perform an installation via rootfs (& add docs on how to)
