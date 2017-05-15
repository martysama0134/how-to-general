
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

1. Open the ssh config file:

	```sh
	$ ee /etc/ssh/sshd_config
	```

2. Edit/Add the following options as such:

	```sh
	RSAAuthentication yes
	PubkeyAuthentication yes
	AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2
	```

3. Refresh the settings:

	```sh
	$ service sshd reload
	```

	_Note: by doing `reload` instead of `restart`, the already connected ssh connections will stay connected and working_

4. Generate and Add a random RSA key following the next steps:

	1. Generate the 4096 bits rsa key:

		```sh
		$ ssh-keygen -t rsa -b 4096
		```
		_Note: `Generating public/private rsa key pair.` can take few seconds_

	2. It will ask for the output name, but leave it empty and press ENTER:

		_`Enter file in which to save the key (/root/.ssh/id_rsa):`_

	3. It will ask for the passphrase: (min #pass>4 character length)

		_`Enter passphrase (empty for no passphrase):`_

		_`Enter same passphrase again:`_

		_Note: using a passphrase instead of leaving blank is highly suggested!_

	4. Now you have two files inside `/root/.ssh`: `id_rsa` and `id_rsa.pub`. Save both of them in your computer.

		_Note: you can ignored the printed key fingerprint and key's randomart image._

	5. Add the public key in the authorized keys:

		```sh
		$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys2
		```

		_Note: `~` is the name of the current user. If root, it will be `/root`.

	6. Now delete the generated keys: (after you saved a copy of them in your computer)

		```sh
		$ rm ~/.ssh/id_rsa ~/.ssh/id_rsa.pub
		```

	_Note: you don't need to refresh anything to enable and use the new key._

	_Note2: you can use any name instead of `id_rsa`._

---
Some applications (winscp/navicat) require a private key in `ppk` format. You can do so by:

1. Installing puttygen.exe (inside the putty package)
2. Opening it, and loading the private key `id_rsa` by doing **Conversions -> Import key**

	[![Result Label](http://i.imgur.com/hrftY9G.png)](http://i.imgur.com/hrftY9G.png)

	[![Result Label](http://i.imgur.com/C57tzS8.png)](http://i.imgur.com/C57tzS8.png)

3. Save the generated key by pressing the button **Save private key** as `id_rsa.ppk`

	[![Result Label](http://i.imgur.com/BBFRvM1.png)](http://i.imgur.com/BBFRvM1.png)

	[![Result Label](http://i.imgur.com/ei5Rfx3.png)](http://i.imgur.com/ei5Rfx3.png)

---
To log in with WINSCP:

1. Open the relative server's settings
2. Don't specify any password (must be blank)
3. Specify the `id_rsa` key in **Advanced -> SSH -> Authentication**

	_Note: It will automatically convert the `id_rsa` key to `id_rsa.ppk`_

---
To log in with NAVICAT: (implies you use a ssh tunnelling)

1. Open the **Connection Properties** of the specified server
2. In the SSH tab you specify:
	1. Authentication Method: **Public Key**
	2. Private Key: **id_rsa.ppk`
---
