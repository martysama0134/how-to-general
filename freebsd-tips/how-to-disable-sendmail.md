# How to disable Sendmail

1. Open `/etc/rc.conf`:

	```sh
	$ ee /etc/rc.conf
	```

2. Edit/Add these options:

	```
	# NO SENDMAIL
	sendmail_enable="NO"
	sendmail_submit_enable="NO"
	sendmail_outbound_enable="NO"
	sendmail_msp_queue_enable="NO"
	```

3. Stop the `sendmail` service:

	```sh
	service sendmail stop
	```

The main reason why someone would disable the sendmail service are usually:

1. If you're using a crappy vps, it will take some hard disk usage.
2. For better alternatives
3: For minimal systems

By default sendmail should work only locally.
