# How to load the IPFW rules

1. Copy your `ipfw.rules` inside /etc/

	```sh
	$ cp ipfw.rules /etc/
	```

	_Note: Be sure the rules don't have \r\n lines (CRLN windows lines), but only \n lines (Unix LN lines), otherwise you get locked out!_

1. Open `/etc/rc.conf`:

	```sh
	$ ee /etc/rc.conf
	```

1. Edit/Add these options:

	```
	# IPFW
	firewall_enable="YES"
	firewall_type="open"
	firewall_script="/etc/ipfw.rules"
	firewall_logging="YES"
	```

	_Note: For testing, set the `firewall_enable` to "NO"_

1. Start the ipfw service:

	```sh
	$ service ipfw start
	```

	_Note: if `firewall_enable` is set to "NO", you should use `onestart|onestop|onerestart` instead of `start|stop|restart`_

If you're afraid of getting locked out, you can try this way:

1. Use the `firewall_enable="NO"` option inside `/etc/rc.conf`
1. Start the service with the command `service ipfw onestart && sleep 60 && service ipfw onestop &` (ipfw will be stopped after 60 seconds)
