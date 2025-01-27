# Configuração SSL Auto Assinado para Syspass

1.0. Instale o Openssl:

    $ sudo apt install openssl

2.0. Gere uma chave privada e o certificado auto assinado do site:
    - commom_name é onde digita o ip ou o nome do host 
    
    $ openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out nginx-certificate.crt -keyout nginx.key




1.0. Ativando o modo ssl e rewrite:

    $ a2enmod ssl
    $ a2enmod rewrite














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
