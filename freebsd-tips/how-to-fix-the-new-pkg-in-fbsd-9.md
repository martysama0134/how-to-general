# How to fix the new pkg in Freebsd 9
In fbsd 9, there are two kind of package management tools, pkg and pkg-tools.

`pkg` is the new one, and has been installed without any configuration, which is why it doesn't work most of the times.

To fix it, we need to configure it manually:

1. We replace the `pkg.conf` file with the sample:

	```sh
	$ mv /usr/local/etc/pkg.conf /usr/local/etc/pkg.conf.old
	$ cp /usr/local/etc/pkg.conf.sample /usr/local/etc/pkg.conf
	```

2. We add the allowed pkg key in the trusted ones

	```sh
	$ mkdir -p /usr/share/keys/pkg/trusted /usr/share/keys/pkg/revoked
	$ cd /usr/share/keys/pkg/trusted
	$ fetch http://svn0.us-west.freebsd.org/base/head/share/keys/pkg/trusted/pkg.freebsd.org.2013102301
	```

3. We remake the relative pkg repository config file, by editing the following files with

	```sh
	$ ee /etc/pkg/FreeBSD.conf
	```

	And pasting the following stuff:

	```
	# $FreeBSD$
	#
	# To disable this repository, instead of modifying or removing this file,
	# create a /usr/local/etc/pkg/repos/FreeBSD.conf file:
	#
	#   mkdir -p /usr/local/etc/pkg/repos
	#   echo "FreeBSD: { enabled: no }" > /usr/local/etc/pkg/repos/FreeBSD.conf
	#

	FreeBSD: {
	  url: "pkg+http://pkg.FreeBSD.org/${ABI}/latest",
	  mirror_type: "srv",
	  signature_type: "fingerprints",
	  fingerprints: "/usr/share/keys/pkg",
	  enabled: yes
	}
	```

4. We then force an update check:

	```sh
	$ pkg update -f
	```

---

## Note:

1. `pkg.freebsd.org.2013102301` is a simple textual file containing: (with a newline at the end)

	```
	# $FreeBSD$

	function: "sha256"
	fingerprint: "b0170035af3acc5f3f3ae1859dc717101b4e6c1d0a794ad554928ca0cbb2f438"
	```
