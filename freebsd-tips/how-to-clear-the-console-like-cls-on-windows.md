# How to clear the console like cls on Windows

1. Open `/usr/local/bin/cls`:

	```sh
	$ ee /usr/local/bin/cls
	```

1. Paste:

	```sh
	#!/bin/sh
	clear
	printf '\033[3J'
	```

1. Give permission:

	```sh
	$ chmod u+x /usr/local/bin/cls
	```

_Note: this is especially done for PuTTy._
