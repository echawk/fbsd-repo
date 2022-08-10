# fbsd-repo

![downloads](https://img.shields.io/github/downloads/ehawkvu/fbsd-repo/total.svg)

An experimental kiss repo for [FreeBSD](https://freebsd.org).

## Installation (WIP)

The installation currently requires you to hijack an already existing
FreeBSD installation. This will change in the future.

On the FreeBSD machine (and as root), install git with `pkg`:

```shell
# Install git, since it pulls in curl.
yes | pkg install git

export KISS_SU=sudo
export MAKEFLAGS="$(sysctl -n hw.ncpu)"

git clone https://github.com/ehawkvu/fbsd-repo
git clone https://github.com/kiss-community/kiss

export PATH="$PWD/kiss:$PATH"
export KISS_PATH="$PWD/fbsd-repo/core"
export KISS_HOOK="$PWD/fbsd-repo/hook"
kiss d bsd-base curl git gmake kiss

# Update certdata so our curl works
sh $PWD/fbsd-repo/openssl/files/update-certdata.sh

# Remove stuff from pkg.
rm -Rf /var/db/pkg/*
rm -Rf /usr/local/*

kiss b curl
kiss b gmake
kiss b git
kiss b kiss

# Do NOT install this package yet, there's still plenty of
# issues with it (see below).
# You're welcome to build it, however.
kiss b bsd-base
```

### Generating rootfs

With the above steps taken, the rootfs can be generated, however
there is a manual step that you will have to perform, as noted below.


## Current error:

Currently kiss can *build* bsd-base just fine, however it cannot *install* it as of yet.

Installation of bsd-base required the `cp` calls in `pkg_install()` in `kiss` to be followed by `|| true` just so that installation could finish successfully. There are many symlinks that just fail to install properly with this particular package, here's the errors:

```
cp: symlink: ../sbin/mailwrapper: File exists
cp: symlink: ../sbin/mailwrapper: File exists
cp: symlink: ../usr/sbin/nologin: File exists
cp: symlink: ../../bin/grep: File exists
cp: symlink: ../../bin/pkill: File exists
cp: cannot overwrite directory /tmp/rootfs/usr/lib/./include with non-directory /root/.cache/kiss/proc/50870/extract/bsd-base/usr/lib/include
cp: symlink: ../../libexec/ld-elf.so.1: File exists
cp: symlink: ../../libexec/ld-elf32.so.1: File exists
cp: cannot overwrite directory /tmp/rootfs/usr/share/nls/./POSIX with non-directory /root/.cache/kiss/proc/50870/extract/bsd-base/usr/share/nls/POSIX
cp: cannot overwrite directory /tmp/rootfs/usr/share/nls/./en_US_ASCII with non-directory /root/.cache/kiss/proc/50870/extract/bsd-base/usr/share/nls/en_US.US_ASCII
cp: symlink ../local/tests: File exists
cp: cannot overwrite directory /tmp/rootfs/usr/test/sys/pjdfstest/./tests with non-directory /root/.cahe/kiss/proc/50870/extract/bsd-base/usr/tests/pjdfs/tests
cp: symlink: ../sbin/mailwrapper: File exists
cp: symlink: ../sbin/mailwrapper: File exists
cp: symlink: ../usr/sbin/nologin: File exists
cp: symlink: ../../bin/pgrep: File exists
cp: symlink: ../../bin/pkill: File exists
cp: cannot overwrite directory /tmp/rootfs/usr/lib/./include with non-directory /root/.cache/kiss/proc/50870/extract/bsd-base/usr/lib/include
cp: symlink: ../../libexec/ld-elf.so.1: File exists
cp: symlink: ../../libexec/ld-elf32.so.1: File exists
cp: cannot overwrite directory /tmp/rootfs/usr/share/nls/./POSIX with non-directory /root/.cache/kiss/proc/50870/extract/bsd-base/usr/share/nls/POSIX
cp: cannot overwrite directory /tmp/rootfs/usr/share/nls/./en_US_ASCII with non-directory /root/.cache/kiss/proc/50870/extract/bsd-base/usr/share/nls/en_US.US_ASCII
cp: symlink: ../local/tests: File exists
cp: cannot overwrite directory /tmp/rootfs/usr/test/sys/pjdfstest/./tests with non-directory /root/.cahe/kiss/proc/50870/extract/bsd-base/usr/tests/pjdfs/tests
```

Yes the repeat lines did occur in the actual output. This is
the exact log. No, I don't know what caused it.


## TODO
* [ ] Use a git module for the upstream kiss repository, so less maintenance is required.
* [x] Generate a rootfs
* [ ] Make sure you can perform an installation via rootfs (& add docs on how to)
