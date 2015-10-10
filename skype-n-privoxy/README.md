###### [How-To] Privoxy Server Creation + Skype behind Proxy

---
# Table of Contents

* [1.0 Intro](#10-intro)
* [1.1 Minimal Privoxy server creation (on FreeBSD)](#11-minimal-privoxy-server-creation-on-freebsd)
* [1.2 Installation](#12-installation)
* [1.3 Configuration](#13-configuration)
* [1.4 Starting Up the Proxy](#14-starting-up-the-proxy)
* [1.5 Setting Skype up for the proxy](#15-setting-skype-up-for-the-proxy)

---
## 1.0 Intro

This guide will explain the following two things:

* Minimal Privoxy server creation
* Using skype behind a proxy server

---
## 1.1 Minimal Privoxy server creation (on FreeBSD)

Even though the explanation will cover only the configuration on FreeBSD, you can use Privoxy on any unix-based systems.

Privoxy is a simple solution to create a proxy server.

We could also use Squid or other alternatives, but it would be time-consuming trying to use 'em for such a trivial reason.

---
## 1.2 Installation

You can easily install Privoxy from the pre-compiled packages:

```
# pkg search privoxy
# pkg install privoxy-454353
```

---
## 1.3 Configuration

Now, you need to specify both IP and Port to use inside the file "/usr/local/etc/privoxy/config".

Find this line:

`listen-address  127.0.0.1:8118`

and change it with:

`listen-address  111.222.333.444:666`

_Note: 111.222.333.444 should be the real machine's IP, and 666 the relative port used by privoxy to listen all the incoming connections._

---
## 1.4 Starting Up the Proxy

You should now run the privoxy service, and start using it.
You have two ways:

* Opening "/usr/rc.conf" and adding the following line inside of it:

  `privoxy_enable="YES"`

  Then you can start the service doing:

  `# service privoxy start`

  _Note: Adding such a line will let the system run privoxy at boot time._

* Simply:

  `service privoxy onestart`

  _Note: You will have to run this every time you reboot your server._

The proxy is now ready to be used.

---
## 1.5 Setting Skype up for the proxy

As you could imagine, Skype has tons of issues.

Even the proxy settings have horror stories.

If you dare to use the **Options->Advanced->Connection** option for it, you'll be reconnected through your real IP if skype can't connect to the proxy.

_tl;dr If someone ddosses your proxy, your skype will show your real IP to your contacts, and then they could directly attack you._

Luckily, Skype supports an alternative and concealed way through windows registry modifications. (??? rly)

_tl;dr If your proxy is down, your skype will stay totally offline, and your real IP won't be shown to anyone._

You need to create a .reg file, run it and restart Skype.exe, and everything will go fine.

You can use [this site](https://dl.dropboxusercontent.com/u/33446/twitch/skype.html) (suggested by the EdgyShadow) to auto-create such file, otherwise you can do it manually.

I usually use these three files to enable/disable the proxy on skype, or either use HTTPS or SOCKS:

* [Skype__AddProxy_HTTPS.reg](./Skype__AddProxy_HTTPS.reg)
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Skype\Phone]
"DisableSupernode"=dword:00000001
"ProxySetting"="HTTPS"
"DisableUDP"=dword:00000001
"ProxyAddress"="111.222.333.444:666"
"ProxyUsername"=-
"ProxyPassword"=-
```

* [Skype__AddProxy_SOCKS5.reg](./Skype__AddProxy_SOCKS5.reg)
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Skype\Phone]
"DisableSupernode"=dword:00000001
"ProxySetting"="SOCKS5"
"DisableUDP"=-
"ProxyAddress"="111.222.333.444:666"
"ProxyUsername"=-
"ProxyPassword"=-
```

* [Skype__RemoveProxy.reg](./Skype__RemoveProxy.reg)
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Skype\Phone]
"DisableSupernode"=-
"ProxySetting"=-
"ProxyAddress"=-
"DisableUDP"=-
"ProxyUsername"=-
"ProxyPassword"=-
```

I have used this configuration since 2013, and I never got any problems with it. The rest is up to you.

_Note: Privoxy doesn't support user authentication, but you can set a range of allowed IPs from the setting file._
