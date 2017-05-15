
---
# Table of Contents
* [Abilitate root login](#abilitate-root-login)
* [Switch SSH port](#switch-ssh-port)
* [How to create an RSA ssh key](#how-to-create-an-rsa-ssh-key)

--------------------------------------------------------------------------------
# Abilitate root login
By default, when installing the OS, root can't login via ssh, so we need to enable it.

1. Open the ssh config file:

	```sh
	$ ee /etc/ssh/sshd_config
	```

2. Edit/Add the following option as such:

	```sh
	PermitRootLogin yes
	```

	_Note: by default, it's `no`. There's also another option `without-password` for enabling just login via rsa ssh key and disabling via password._

3. Refresh the settings:

	```sh
	$ service sshd reload
	```

	_Note: by doing `reload` instead of `restart`, the already connected ssh connections will stay connected and working_


--------------------------------------------------------------------------------
# Switch SSH port
By default, the (sftp) ssh port is 22, but it's easily swichable.

1. Open the ssh config file:

	```sh
	$ ee /etc/ssh/sshd_config
	```

2. Edit/Add the following option as such specifying a different port number: (it's advised to use a number above 1024 since the first 1024 are reserved)

	```sh
	Port 22
	```

3. Refresh the settings:

	```sh
	$ service sshd reload
	```

	_Note: by doing `reload` instead of `restart`, the already connected ssh connections will stay connected and working_


--------------------------------------------------------------------------------
# How to create an RSA ssh key
Moved in [Here](how-to-create-and-setup-an-ssh-key.md)
