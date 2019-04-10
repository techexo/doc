# Install wallabag on your own server (or a dedicated server)

{% hint style="info"}
If you're here, it means that you have a Linux/UNIX server running with [all the requirements met](readme.md) and an administrator access to it (`root` or `www-data` privileges). If you plan to install wallabag on a commercial web hosting service, please refer to the [relevant section of the documentation](installation_webhosting.md).
{% endhint %}

Our prefered way to install wallabag is using `git`, `make`, and `composer`: the last version will be downloaded and configured, and following upgrades should be going smoothly. `composer` also allows to check if requirements are met before trying to run wallabag.

## Download

Download wallabag in the desired directory (probably `/var/www`):
```
   $ git clone https://github.com/wallabag/wallabag.git /var/www/wallabag
```

## Install wallabag
1. Enter wallabag's directory:
```
   $ cd /var/www/wallabag
```

2. Run the installer **not as `root`**:
```
   $ make install
```

{% hint style="info" %}
This command will download dependencies and prepare the configuration file of wallabag. You will need to have a working database at this point, and we will consider that you created on `localhost` a `wallabag` user having all privileges over the `wallabag` database.

The SQL request could be something like that `GRANT ALL PRIVILEGES ON wallabag.* TO 'wallabag'@'localhost' IDENTIFIED BY 'password';`, but we refer you to your database manual ([MariaDB](https://mariadb.com/kb/en/library/grant/#identified-by-password) or [PostgreSQL](https://www.postgresql.org/docs/11/sql-grant.html) and your favorite search engine for more information.
{% endhint %}

3. Wait for the dependencies to be downloaded, then answer the questions. We think the names of the asked parameters are self-explanatory and the default (and sensible?) values are indicated between parenthesis during the installation. However, a detailed list of the parameters [is available help should be needed](../parameters.md).

## Fix permissions

For wallabag to run smoothly, some directories have to be read and/or written by the web server. Consequently, ownership of some directories have to be transfered to the web server's user, which is `www-data` on most Linux distributions (you might want to check that it is indeed the case for yours).

If you cloned wallabag and ran the installer as `www-data` (prefixing all commands by `sudo -u www-data`, for example), everything should be fine. Else, you will need to run the following command as `root` (or using `sudo`):
```
cd /var/www/wallabag && chown -R www-data:www-data app/config bin data var vendor web
```

## Configure your web server


## Enjoy!
