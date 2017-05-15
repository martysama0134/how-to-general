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
