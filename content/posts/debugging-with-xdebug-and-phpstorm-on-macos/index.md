+++
title = "Debugging with Xdebug and PhpStorm on macOS"
date = 2020-04-03T11:00:00+01:00
author = "John Svensson"
description = "How to set up debugging with Xdebug and PhpStorm on macOS"
+++

This guide assumes that you've installed PHP with brew. See [macOS 10.15 Catalina Apache Setup: Multiple PHP Versions](https://getgrav.org/blog/macos-catalina-apache-multiple-php-versions) for a good guide on how to do that.

First we need to install Xdebug, we will do that using `pecl`

```bash
$ pecl install xdebug
```

If you run `php -v` in the Terminal you should see Xdebug loaded.

```bash
$ php -v
PHP 7.2.15 (cli) (built: Feb 26 2019 10:52:23) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.15, Copyright (c) 1999-2018, by Zend Technologies
    with Xdebug v2.7.2, Copyright (c) 2002-2019, by Derick Rethans
```

Next we need to configure Xdebug, PHP configuration is located at `/usr/local/etc/php/7.x/conf.d/` when installed with brew. Let's create a ext-xdebug.ini file within that location.

```bash
$ cd /usr/local/etc/php/7.2/conf.d/
$ touch ext-debug.ini
```

Open that file with your favorite text editor.

```ini
[xdebug]
zend_extension="xdebug.so"
xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_handler=dbgp
xdebug.remote_port=9000
xdebug.remote_autostart=1
```

Now you will need to restart your web server (i.e. Apache or built-in web server) for these changes to be applied.

With that being done now we just need to configure PhpStorm. First we will create a new server:

PhpStorm -> Preferences -> Languages & Framework -> PHP -> Servers

{{< image "phpstorm-server.png" "PhpStorm Servers User interface" >}}

Make sure you select Xdebug as the debugger, port and host depends on your local setup. I'm using built-in web server exposed on port 8888. Apply and save.

Finally we will configure Xdebug:

PhpStorm -> Preferences -> Languages & Framework -> PHP -> Debug

{{< image "phpstorm-xdebug.png" "PhpStorm Debug User interface" >}}

Ensure debug port is set to 9000, otherwise the defaults should be fine.

For a more in-depth guide you should see PhpStorm's [Configuring Xdebug](https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html).
Having Xdebug enabled affects performance and you may want to configure only for in-demand or being able to disable it using a command.