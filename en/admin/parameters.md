# Parameters

These parameters are defined during the installation process and stored in `app/config/parameters.yml`. Default values of these parameters are explicited between parenthesis.

{% hint style="tip" %}
Modifying `parameters.yml` after the initial installation is possible and allows one to fix mistakes, adapt the configuration to new services and so on. However, you **must** clear wallabag's cache or the modifications won't be took into account!

In wallabag's directory, run `php bin/console clear:cache -e prod` (you may need to prefix the command with `sudo -u www-data`, according to the permissions of directories).
{% endhint %}

## Database

- **database_driver** (pdo_mysql): `pdo_mysql` if you use MySQL/MariaDB, `pdo_pgsql` if you use PostgreSQL. `pdo_sqlite` can be used for SQLite, but support will be dropped in wallabag 2.4
- **database_driver_class** (null): only useful for PostreSQL 10 (should then be defined to `Wallabag\CoreBundle\Doctrine\DBAL\Driver\CustomPostgreSQLDriver`)
- **database_host** (127.0.0.1): address of your database server (`127.0.0.1` or `localhost` are often the answer)
- **database_port** (null): port of your database server, leave empty for default (MariaDB: 3306, PostgreSQL: 5432)
- **database_name** (wallabag): name of the dedicated database (`wallabag` in this documentation)
- **database_user** (root): user to log on the database server (default is `root`, but we chose `wallabag` in the installation documentation)
- **database_password** (null): password of the `database_user` defined above (`wallabag` in this documentation)
- **database_path** (null): define to `%kernel.root_dir%/../data/db/wallabag.sqlite` if you use SQLite, else let empty. Be careful, SQLite support will be dropped in wallabag 2.4
- **database_table_prefix** (wallabag_): all wallabag's table will be prefixed with this. Let the default value if you don't sknow what you're doing
- **database_socket** (null): if your database is using a socket instead of tcp, put the path of the socket (other connection parameters will then be ignored)
- **database_charset** (utf8mb4): for PostgreSQL & SQLite you should use `utf8`, for MySQL/MariaDB use `utf8mb4` to handle emojis and other special characters

## Domain & Mail

- **domain_name** ('https://your-wallabag-url-instance.com': full URL of your wallabag instance, including the protocol (`http://` or `https://`) but without the trailing slash. See also [web server configuration](virtualhosts.md)
- **mailer_transport** (smtp):
- **mailer_host** (127.0.0.1):
- **mailer_user** (null):
- **mailer_password** (null):
- **from_email** (no-reply@wallabag.org):
- **locale** (en):
- **rss_limit** (50):

## Two-factor authentication

- **secret** (ovmpmAWXRCabNlMgzlzFXDYmCFfzGv):
- **twofactor_auth** (true):
- **twofactor_sender** (no-reply@wallabag.org):

## Public registration

- **fosuser_registration** (true):
- **fosuser_confirmation** (true):

## RabbitMQ

- **rabbitmq_host** (localhost):
- **rabbitmq_port** (5672):
- **rabbitmq_user** (guest):
- **rabbitmq_password** (guest):
- **rabbitmq_prefetch_count** (10):

## Redis

- **redis_scheme** (tcp):
- **redis_host** (localhost):
- **redis_port** (6379):
- **redis_path** (null) :
- **redis_password** (null):
