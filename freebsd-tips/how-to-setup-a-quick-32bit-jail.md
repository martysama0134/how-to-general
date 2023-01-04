# How to setup a quick 32bit jail

This solution is a very quick one. No ezjail, no iocage, only pure FBSD 10+ one.

#### Create folder and fetch the base (adjust the freebsd version in the url):

```sh
$ mkdir -p /home/jails/jailsrc
$ cd /home/jails/jailsrc
$ fetch http://ftp.freebsd.org/pub/FreeBSD/releases/i386/i386/13.1-RELEASE/base.txz
$ tar xpf base.txz;rm -rf boot;rm base.txz
```

#### Copy `resolv.conf`: (if there's no internet inside the jail)

```sh
$ cp /etc/resolv.conf /home/jails/jailsrc/etc/resolv.conf
```

#### Copy [`jail.conf`](res/jail.conf) in `/etc/`: (end lines as \n LF and not \r\n CRLF)
```sh
allow.raw_sockets = 1;
exec.clean;
exec.system_user = "root";
exec.jail_user = "root";
exec.start = "/bin/sh /etc/rc";
exec.stop = "/bin/sh /etc/rc.shutdown";
exec.consolelog = "/var/log/jail_${name}_console.log";
mount.devfs;
allow.mount;
allow.set_hostname = 0;
allow.sysvipc = 0;
path = "/home/jails/${name}";

jailsrc {
	host.hostname = "jailsrc.freebsd.org";
	ip6 = "inherit";
	ip4 = "inherit";
}
```

#### enable jail at boot starting
```sh
$ sysrc jail_enable=yes
```

## Jail Commands
```sh
# start the jail service without enable="YES"
$ service jail onestart
# start the jail service with enable="YES"
$ service jail start
# see the list of the running jails
$ jls
# access into the specific jail <id> shell
$ jexec 1 tcsh
```

## Jail Commands Extra
```sh
### force kill and unmount
$ jail -r 1
$ umount /home/jails/jailsrc/dev

### to reclean the untar
$ cd ..
$ find jailsrc -flags +schg -print -exec chflags noschg {} \;
$ rm -rf jailsrc

### create home2
$ mkdir -p /home/jails/jailsrc/home
$ ln -s /home/jails/jailsrc/home /home2

### to exit from the shell
$ exit
### (or CTRL+D)
```
