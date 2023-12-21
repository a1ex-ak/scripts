# OnlyOffice Document Server
#
    sudo -u www-data php /var/www/nextcloud/occ app:install documentserver_community

# Изменяем в файле конфига
    sudo nano /var/www/nextcloud/config/config.php
    'allow_local_remote_servers' => true,
