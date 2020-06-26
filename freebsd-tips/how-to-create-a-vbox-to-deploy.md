It's mostly for me.

1. Download from [Freebsd](https://www.freebsd.org/releases/) the relative `FreeBSD-<version>-RELEASE-i386-disc1.iso` file.
1. Create a new vm from virtualbox, allocate 2048 (2gb) of ram and 14gb of space.
1. After created the vm,
	close every virtualbox window,
	and open the relative `%userprofile%\VirtualBox VMs\<machinename>\<machinename>.vbox`

	then replace `<NAT/>` with

	```xml
			  <NAT>
				<Forwarding name="Rule 01" proto="1" hostport="13306" guestport="3306"/>
				<Forwarding name="Rule 02" proto="1" hostport="10022" guestport="22"/>
				<Forwarding name="Rule 03" proto="1" hostport="30000" guestport="30000"/>
				<Forwarding name="Rule 04" proto="1" hostport="30001" guestport="30001"/>
				<Forwarding name="Rule 05" proto="1" hostport="30002" guestport="30002"/>
				<Forwarding name="Rule 06" proto="1" hostport="30003" guestport="30003"/>
				<Forwarding name="Rule 07" proto="1" hostport="30004" guestport="30004"/>
				<Forwarding name="Rule 08" proto="1" hostport="30005" guestport="30005"/>
				<Forwarding name="Rule 09" proto="1" hostport="30006" guestport="30006"/>
				<Forwarding name="Rule 10" proto="1" hostport="30007" guestport="30007"/>
			  </NAT>
	```
	1. After that, set the network card as NAT and start the vm.
	1. Choose the .iso for the installation: (if not mentioned, choose the default option)
		1. Choose `localhost.freebsd.org` as hostname
		1. Choose only `doc` as system components (press space for toggling)
		1. Choose `password` as password
		1. Choose every additional option (e.g. disable x y z)
		1. Don't add other users
	1. As additional command after the installation, enable ssh login via root:
		1. `ee /etc/ssh/sshd_config`
		1. write `PermitRootLogin yes`
		1. save with ctrl+c and write "exit"
		1. `shutdown -r now`
1. Unmount the iso disk, and reboot (press Reset)
1. Login
	- host: localhost
	- port: 10022
	- user: root
	- password: password

1. Install the missing packages:
	```sh
	pkg update -f
	pkg install mysql56-server git python gmake gcc9
	```

1. `/etc/rc.conf` should look like this:
	```sh
	clear_tmp_enable="YES"
	syslogd_flags="-ss"
	sendmail_enable="NONE"
	hostname="localhost.freebsd.org"
	ifconfig_em0="DHCP"
	sshd_enable="YES"
	dumpdev="AUTO"
	mysql_enable="NO"
	```

1. Add this setting in order to cache the git credentials on freebsd (until reboot):
	```sh
	git config --global credential.helper "cache --timeout=9993600"
	```

1. Create /home folder:
	```sh
	mkdir /home
	```

1. Find and install the latest gdb version:
	```sh
	pkg search gdb
	pkg install gdb-9.2
	```

1. Close the VM with "send the shutdown signal" option.
1. In Virtualbox window: File -> Export virtual application
	- Format OVF 2.0
	- File with "(clean)"
	- In every Settings info -> My website url

Done.
