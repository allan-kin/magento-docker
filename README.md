# Start the container by running the command:

```docker compose up -d --build```

# Inside the docker php container
(```docker compose exec php bash```)

Execute the following steps:

```
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition .
```

```
bin/magento setup:install \
--base-url=http://localhost \
--db-host=mysql \
--db-name=magento \
--db-user=magento \
--db-password=magento \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1 \
--search-engine=opensearch \
--opensearch-host=opensearch \
--opensearch-port=9200 \
--opensearch-index-prefix=magento2 \
--opensearch-timeout=15 
```

# Setup file permissions:

```
chown -R www-data:www-data /var/www/html
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chmod u+x bin/magento
```

# Setup environment:

```
bin/magento setup:upgrade && \
bin/magento setup:static-content:deploy -f && \
bin/magento cache:flush && \
bin/magento indexer:reindex
```

# Setting local email watcher to read emails:

Create file: 

`touch /etc/msmtprc`

`vim /etc/msmtprc`

```
defaults
auth off
tls off
logfile ~/.msmtp.log

account default
host mailpit
port 1025
from admin@magento.local
```

Magento uses sendmail which dockers doesn't contain, so we need to symlink to pass it to mailpit:

`ln -sf /usr/bin/msmtp /usr/sbin/sendmail`

You can see the emails at `http://localhost:8025`


