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
# cd /var/www/wallabag && chown -R www-data:www-data app/config bin data var vendor web
```

## Configure your web server

Now that wallabag has been downloaded and set up, it is time to configure your web server. It's impossible that we provide a working configuration for all existing web servers, so we will focus on the two most common: Apache2 and nginx.

### Apache2 VirtualHost

If you have an up-to-date version of Apache2 (i.e. version `2.4`), the configuration is pretty straightforward. Please note that wallabag requires `mod_php` to be enabled, and is better with `mod_rewrite` enabled too:
```
  # a2enmod mod_php && a2enmod mod_rewrite
  # systemctl reload apache2
```

#### Access at the root of a domain or a subdomain

In the case you want wallabag to be at the root of your domain or your subdomain (e.g. `http://lvh.me/` or `http://wallabag.lvh.me/`), edit `/etc/apache2/sites-available/wallabag.conf` with the following:
```apache
<VirtualHost *:80>
  # ServerName and ServerAlias should be identical to the
  # domain_name you indicated during wallabag's install
  ServerName wallabag.lvh.me
  ServerAlias wallabag.lvh.me

  # Following paths should be linking to your actual wallabag's installation folder
  DocumentRoot /var/www/wallabag/web
  <Directory /var/www/wallabag/web>
    Require all granted
    AllowOverride FileInfo Indexes Options=MultiViews
  </Directory>

  ErrorLog /var/log/apache2/wallabag_error.log
  CustomLog /var/log/apache2/wallabag_access.log combined
</VirtualHost>
```
{% hint style="info"}
The `AllowOverride` instruction is necessary with Apache2.4 to allow for the execution of wallabag's `.htaccess` file, which contains folder restrictions, URL rewriting rules, and alternative fallback measures should `mod_rewrite` not be available.
{% endhint %}

Enable your virtualhost (`# a2ensite wallabag`), reload Apache2 configuration (`#systemctl reload apache2`) and wallabag should then be available at the URL `http://wallabag.lvh.me/`!

#### Access in a subfolder

If you don't have the possibility to have a sub-domain and already have other applications running on your web server, you might want not to install wallabag at the root of your server, but in a subfolder (e.g. `http://lvh.me/wallabag`).

To do so, edit `/etc/apache2/sites-available/wallabag.conf`:
```apache
<VirtualHost *:80>
  # ServerName and ServerAlias is your domain's root
  ServerName lvh.me
  ServerAlias lvh.me

  DocumentRoot /var/www

  # Replace /wallabag/ by the name of the folder you want wallabag in
  # ServerName + Alias shall be what you indicated as domain_name during the installation
  Alias /wallabag/ /var/www/wallabag/web/
  <Directory /var/www/wallabag/web>
    Require all granted
    AllowOverride FileInfo Indexes Options=MultiViews
  </Directory>

  ErrorLog /var/log/apache2/wallabag_error.log
  CustomLog /var/log/apache2/wallabag_access.log combined
</VirtualHost>
```
{% hint style="info"}
Note here that the `Alias /wallabag/` indicates to Apache2 what it should look for in the URL and has not to be an actual folder. Thus, if you want your wallabag instance to be accessible at `http://lvh.me/opensourceisgreat`, you shall indicate `Alias /opensourceisgreat/`, followed by the actual installation directory of wallabag (here `/var/www/wallabag` followed by `/web/`).
{% endhint %}

Enable your virtualhost (`# a2ensite wallabag`), reload Apache2 configuration (`#systemctl reload apache2`) and wallabag should then be available at the URL `http://lvh.me/wallabag`!

### nginx Server Configuration

As wallabag is written mostly in PHP, we will need being able to execute such code. However, contrary to Apache2, nginx doesn't embed a PHP mod. Instead, it passes everything written in that language to a service that will execute it for him: PHP-FPM. We won't provide a full-on tutorial on this as the web has already many (many many) content on this; we will once again send you back to your favorite search engine (`nginx php` or `nginx php-fpm` are more than sufficient keywords :smile:).

#### Access at the root of a domain or a subdomain

In the case you want wallabag to be at the root of your domain or your subdomain (e.g. `http://lvh.me/` or `http://wallabag.lvh.me/`), edit `/etc/nginx/sites-available/wallabag.conf` with the following:
```nginx
server {
  # Wallabag installation folder, followed by /web
  root /var/www/wallabag/web;
  # Should be identical to domain_name indicated during installation
  server_name wallabag.lvh.me;
  listen 80;

  location / {
    try_files $uri /app.php$is_args$args;
  }

  location ~ ^/app\.php(/|$) {
    # This is the link towards your PHP-FPM socket
    fastcgi_pass unix:/run/php/php7.2-fpm.sock;
    fastcgi_read_timeout 300;
    include snippets/fastcgi-php.conf;
      fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
      fastcgi_param DOCUMENT_ROOT $realpath_root;

      # Prevents URIs that include the front controller. This will 404:
      # http://domain.tld/app.php/some-path
      # Remove the internal directive to allow URIs like this
      internal;
  }

  error_log /var/log/nginx/wallabag.error.log;
  access_log /var/log/nginx/wallabag.access.log;
}
```

Enable the site configuration (`# ln -s /etc/nginx/sites-available/wallabag.conf /etc/nginx/sites-enabled/wallabag.conf`), reload nginx (`# systemctl reload nginx`), then wallabag should be available at `http://wallabag.lvh.me`!

#### Access in a subfolder

Due to the use of PHP-FPM with nginx, it is quite more difficult to allow the installation of wallabag in a subfolder using this webserver. It is probably doable, but we weren't able to find a proper configuration. If you know how to do it, do not hesitate to ping us on [Github](https://github.com/wallabag/doc/issues) :pray:!

### TLS Configuration

We encourage you to enable TLS encryption of your communications with the server, to use HTTPS access to your wallabag instance (as you should with all services you're hosting!).

It is however outside the scope of this tutorial to explain everything related to this, so we encourage you to look for more information about it on your favorite search engine. (`apache2/nginx SSL Configuration` or `apache2/nginx and TLS` are good keywords). Also, have a look at [Let's Encrypt](https://letsencrypt.org/getting-started/) if you need TLS certificates!

## Enjoy!

With all this information, you should now be able to enjoy your instance of wallabag on your server! If something is not going as it should, you might want to look at [common errors](../common_errors.md).

Do not hesitate to let us know on the [documentation Github's repository](https://github.com/wallabag/doc/issues) if something was not clear in the doc, or to seek help for installation on [Gitter](https://gitter.im).
