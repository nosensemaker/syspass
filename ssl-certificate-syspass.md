# Configuração SSL Auto Assinado para Syspass

1.0. 













<VirtualHost *:80>
    ServerName syspass.vigilante.kira.br
    Redirect permanent / https://syspass.vigilante.kira.br/
</VirtualHost>


<VirtualHost *:443>
    ServerAdmin admin@localhost
    DocumentRoot "/var/www/html/syspass"
    ServerName syspass.vigilante.kira.br
    SSLEngine on
    SSLCertificateFile /etc/apache2/certificate/syspass.crt
    SSLCertificateKeyFile /etc/apache2/certificate/syspass.key
<Directory "/var/www/html/syspass/">
    Options MultiViews FollowSymlinks
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
    TransferLog /var/log/apache2/syspass_access.log
    ErrorLog /var/log/apache2/syspass_error.log
</VirtualHost>
