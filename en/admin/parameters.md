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

- **domain_name** ('https://your-wallabag-url-instance.com'): full URL of your wallabag instance, including the protocol (`http://` or `https://`) but without the trailing slash.
- **mailer_transport** (smtp): the exact transport method to use to deliver emails. Valid values are smtp, gmail, mail, sendmail, or null (which will disable the mailer)
- **mailer_host** (127.0.0.1): the host to connect to when using smtp as the transport.
- **mailer_user** (null): username when using SMTP transport.
- **mailer_password** (null): password of the username used for SMTP transport.
- **from_email** (no-reply@wallabag.org): email address used in `From:` field in each email.
- **locale** (en): default language of your wallabag instance (en, fr, es...).
- **rss_limit** (50): item limit for RSS feeds.

## Two-factor authentication

- **secret** (ovmpmAWXRCabNlMgzlzFXDYmCFfzGv): this is a string that should be unique to your application and it's commonly used to add more entropy to security related operations.
- **twofactor_auth** (true): enable the possibility of two-factor authentication.
- **twofactor_sender** (no-reply@wallabag.org): email from which two-factor codes will be sent.

## Public registration

- **fosuser_registration** (true): enable public registration (anyone can register to your instance).
- **fosuser_confirmation** (true): send an e-mail confirmation for each registration.

## RabbitMQ

RabbitMQ can be used in order to launch asynchronous tasks, which is mostly useful for huge imports operations. More [here](asynchronous.md#install-rabbitmq-for-asynchronous-tasks).
- **rabbitmq_host** (localhost): host of your RabbitMQ.
- **rabbitmq_port** (5672): port of the RabbitMQ instance.
- **rabbitmq_user** (guest): user authorized to read RabbitMQ queues.
- **rabbitmq_password** (guest): password of that user.
- **rabbitmq_prefetch_count** (10): number of wallabag actions to consider at once.

## Redis

Redis can be used in order to launch asynchronous tasks, which is mostly useful for huge imports operations. More [here](asynchronous.md#install-redis-for-asynchronous-tasks).
- **redis_scheme** (tcp): specifies the protocol used to communicate with an instance of Redis. Valid values are: tcp, unix, http.
- **redis_host** (localhost): host of your Redis instance.
- **redis_port** (6379): port of the Redis instance.
- **redis_path** (null): path of the UNIX domain socket file used when connecting to Redis using UNIX domain sockets.
- **redis_password** (null): password defined in the Redis server configuration (`requirepass` in `redis.conf`).
