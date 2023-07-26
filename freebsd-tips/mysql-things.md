---
# Table of Contents
* [Install MySQL](#install-mysql)
* [Default root password](#default-root-password)
* [Enable query logs](#enable-query-logs)
* [Move the mysql data folder](#move-the-mysql-data-folder)
* [Old password solution for MySQL 8](#old-password-solution-for-mysql-8)
* [Allow connections outside localhost](#allow-connections-outside-localhost)

--------------------------------------------------------------------------------
# Install MySQL
Follow the following commands:

```sh
# update the pkg archive list
$ pkg update -f

# install the mysql server package
$ pkg install mysql56-server

# fix the mysql user in case of corrupted user creation
$ pwd_mkdb -p /etc/master.passwd

# give the mysql privileges to the mysql folders
$ chown -R mysql /var/db/mysql/
$ chgrp -R mysql /var/db/mysql/

# enable mysql at boot starting
$ sysrc mysql_enable=yes

# start the mysql server service
$ service mysql-server start
```

_Note: in the command it's specified mysql 5.6, but you can use the version you want._

_Note2: in MySQL 8.0, inside `/usr/local/etc/mysql/my.cnf`, comment with "#" the parameter line `bind-address = 127.0.0.1` to enable non-localhost connections!_

--------------------------------------------------------------------------------
# Default root password
The default `root@localhost` password is usually blank, but since MySQL 5.7 it's randomly generated after the `mysql-server` service is started for the first time.

The randomly generated password is written inside `~/.mysql_secret`, and we can get it by doing:

```sh
$ head -2 ~/.mysql_secret | tail -1
```

We can't process any query until we change the password, so we do:

```sh
$ mysqladmin -uroot -p password
Enter password:
New password:
Confirm new password:
```

--------------------------------------------------------------------------------
# Enable query logs
Mysql by default doesn't enable any logging for the queries.

To achieve that, we have two ways to do so:

* Via Mysql Table `mysql.general_log`
* Via Logging File `mysql_general.log`

You can either decide if you want it temporary or permanent.

* Temporary:

	You simply need to run a mysql query to set two global variables:

	* Via Mysql Table

		```sql
		SET global log_output = 'table';
		SET global general_log = 1;
		```

	* Via Logging File

		```sql
		SET global log_output = 'file';
		SET global general_log_file = '/var/db/mysql/mysql_general.log';
		SET global general_log = 1;
		```

* Permanent:

	You need to specify it in the `my.cnf` and restart the mysql-server service.

	_Note: The `my.cnf` file isn't created by default when installing MySQL on freebsd. Its common path is `/etc/my.cnf` (mariadb reads it only from `/usr/local/etc/my.cnf`)_

	Add in the `my.cnf` file the following stuff:

	* Via Mysql Table

		```ini
		[mysqld]
		log_output = table
		general_log = on
		```

	* Via Logging File

		```ini
		[mysqld]
		log_output = file
		general_log_file=/var/db/mysql/mysql_general.log
		general_log = on
		```

	And restart the service:

	```sh
	$ service mysql-server restart
	```

_Note: In case you decide to write the logs in the files, be sure the user `mysql` has the permission to write in that specific path._

_Note2: To disable the logging, be sure general_log is simply set to `0` via query or to `off` via config._

--------------------------------------------------------------------------------
# Move the mysql data folder
By default, the mysql data folder is located to /var/db/mysql (it's created when the mysql-server starts, if missing).

To switch the path, you just need to specify it inside `/etc/rc.conf` as such:

```ini
mysql_dbdir="/home/mysql/"
```

then restart the service:

```sh
$ service mysql-server restart
# give the privileges to the mysql group & user privileges for the new mysql folder
$ chown -R mysql /home/mysql/
$ chgrp -R mysql /home/mysql/
```

--------------------------------------------------------------------------------
# Old password solution for MySQL 8
Since MySQL8, the `PASSWORD()` function has been removed, and should replaced with new alternatives.
The equivalent of the old one is:
```sql
SELECT CONCAT('*', UPPER(SHA1(UNHEX(SHA1('password')))));
```

# Allow connections outside localhost
If you're connecting remotely and you get the error `error 2013 - lost connection to server at 'handshake: reading initial communication packet', system error: 10061` then you must allow outside connections.

Locate your `my.cnf` file (usually inside `/usr/local/etc/mysql`) otherwise just use `find / -name my.cnf` to find it.

Inside `my.cnf` add or change the following line related to `bind-address`:

```ini
[mysqld]
bind-address = 0.0.0.0
```

Alternatively, the my.cnf is loading the content from `/usr/local/etc/mysql/conf.d/server.cnf`, so you must edit `bind-address` from there.

After that, you restart the service with `service mysql-server restart`.
