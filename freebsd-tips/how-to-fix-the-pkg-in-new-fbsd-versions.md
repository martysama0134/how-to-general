# How to fix the pkg in new Freebsd versions

In case you got this issue:
```
Updating FreeBSD repository catalogue...
Fetching meta.txz: 100%    916 B   0.9kB/s    00:01
pkg: repository meta has wrong version 2
repository FreeBSD has no meta file, using default settings
Fetching packagesite.txz: 100%    6 MiB 247.5kB/s    00:26
pkg: repository meta has wrong version 2
pkg: Repository FreeBSD load error: meta cannot be loaded No error: 0
Unable to open created repository FreeBSD
Unable to update repository FreeBSD
Error updating repositories!
```

You can solve it with:
```
$ pkg bootstrap -f
$ pkg update -f
```
