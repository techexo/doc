# Installing wallabag

Anyone can run a wallabag instance on its server, as long as you have access to one, are capable of using a terminal, and have a little time in front of you. However, it is to be noted that the installation process is currently not perfectly automated, so a certain comfort with Linux/Unix administrative tools is more than welcome! :-)

## Alternatives to self-hosting wallabag

### wallabag.it

A paid wallabag instance is provided on [wallabag.it](https://wallabag.it) for people that are not willing to install it themselves on a server. This service always ships the latest release of wallabag.

[You can create your account here](https://app.wallabag.it/) and try it for free: you'll get a 14-day free trial with no limitation (no credit card information required).

### Installation with Docker

For Docker aficionados, we provide you a Docker image to install wallabag easily. Have a look at our repository on [Docker Hub](https://hub.docker.com/r/wallabag/wallabag/) for more information.

### Other packages

wallabag is available as packages for [Cloudron](https://cloudron.io/store/org.wallabag.cloudronapp.html), [YunoHost](https://install-app.yunohost.org/?app=wallabag2) and [Synology NAS](https://synocommunity.com/package/wallabag).

{% hint style="danger" %} These packages are not provided directly by the developers of wallabag, and may not be up-to-date! {% endhint %}

## Hosting wallabag: Requirements

If you're not scared, bravo! And let's get to it...

wallabag is an application written (mostly) in PHP, which uses an SQL database. You'll consequently need:

* a running web server: Apache2, nginx, lighttpd...
* an SQL database: MySQL/MariaDB or PostgreSQL (support for SQLite will be dropped with wallabag `2.4`, so we won't include it in this documentation),
* PHP **`>= 5.6` with the following extensions activated**: `ctype`, `curl`, `dom`, `gd`, `hash`, `iconv`, `json`, `mbstring`, `pcre`, `pdo`, `session`, `simplexml`, `tidy`, `tokenizer`, and `xm` (you can see which extensions are on your system using the `php -m` command. Note that PHP `7.3` is not yet officially supported.),
* the PHP extension for your desired SQL database: `pdo_mysql` or `pdo_pgsql`,
* the `make` tool to exploit wallabag's `Makefile` (installed on most Linux/UNIX distributions),
* the `git` software to allow for a simple installation and easy upgrades.

It is outside the scope of this documentation to learn how to install all these softwares, but you can find *lots* of information about them on the internet. Search for "LAMP" (Linux/Apache/MySQL/PHP) or "LNMP/LEMP stacks" (Linux/nginx/MySQL/PHP) on your favorite research engine.

{% hint style="info"} If you are planning to host wallabag on a shared/mutualized server, you might not have to bother with all these details, as a web server, a database and PHP will probably be already installed. However, you need to check with your provider what PHP extensions are installed! Unfortunately, if some of them are missing, you won't be able to install wallabag.

More details for shared hosting are available [later in this documentation](installation_webhosting.md).{% endhint %}

Last but not least, wallabag rely on a large number of external PHP libraries for its functioning, and uses `composer` to manage all the dependencies. You can either install it using your distribution's package manager (please check that it's at least the `1.2` version), or install it locally launching:
```
   curl -s https://getcomposer.org/installer | php
```
You can find more information about `composer` and its installation [on the official website](https://getcomposer.org/doc/00-intro.md).

{% hint style="info"}Once again, if you want to run wallabag on a mutualized server or with a simple web hosting service, you might not have `composer` and won't be able to install it. We provide for this case [a package containing all the dependencies](https://wllbg.org/latest-v2-package); please refer to the relevant [installation section](installation_webhosting.md).{% endhint %}

Once you checked that all the requirements are met, you can follow with the actual installation of wallabag, in the next section(s)!
