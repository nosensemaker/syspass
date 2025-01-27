
Instalação do sysPass no Ubuntu 22.0
Introdução

O sysPass é uma ferramenta de gerenciamento de senhas baseada na web, projetada para oferecer segurança e facilidade de uso. Este guia detalha os passos necessários para instalar o sysPass em um servidor Ubuntu 22.04.

Pré-requisitos

Antes de começar, certifique-se de ter:

Um servidor com Ubuntu 22.04.

Acesso root ou um usuário com privilégios administrativos.

Conexão com a internet.

Certificado SSL (opcional, mas recomendado para maior segurança).



---

Passo 1: Instalar Apache2 e MariaDB

Instale o servidor web Apache2 e o banco de dados MariaDB:

apt-get install apache2 mariadb-server -y


---

Passo 2: Instalar os pacotes necessários

Instale pacotes adicionais e adicione o repositório para o PHP:

apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
add-apt-repository ppa:ondrej/php -y


---

Passo 3: Instalar o PHP e suas extensões

Instale o PHP 7.4 e as extensões necessárias para o sysPass:

apt install libapache2-mod-php7.4 php7.4 php7.4-mysqli php7.4-pdo php7.4-cgi php7.4-cli php7.4-common php7.4-gd php7.4-json php7.4-readline php7.4-curl php7.4-intl php7.4-ldap php7.4-xml php7.4-mbstring git -y

Edite o arquivo de configuração do PHP:

vim /etc/php/7.4/apache2/php.ini

Reinicie o Apache para aplicar as mudanças:

systemctl restart apache2


---

Passo 4: Configurar o banco de dados MariaDB

1. Execute o script de configuração do MariaDB:

mysql_secure_installation


2. Acesse o MariaDB:

mysql -u root -p


3. Crie o banco de dados e configure o usuário:

create database syspassdb;
grant all privileges on syspassdb.* to 'syspassuser'@'localhost' identified by 'password';
flush privileges;
exit;




---

Passo 5: Clonar o repositório do sysPass

Clone o repositório oficial do GitHub:

git clone https://github.com/nuxsmin/sysPass.git

Mova o sysPass para o diretório web:

mv sysPass /var/www/html/syspass

Ajuste as permissões e o proprietário do diretório:

chown -R www-data:www-data /var/www/html/syspass
chmod 750 /var/www/html/syspass/app/{config,backup}


---

Passo 6: Configurar o Composer

Crie o script install-composer.sh no diretório do sysPass:

vim /var/www/html/syspass/install-composer.sh

Adicione o seguinte conteúdo:

#!/bin/sh
EXPECTED_SIGNATURE="$(wget -q -O - https://composer.github.io/installer.sig)"
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
ACTUAL_SIGNATURE="$(php -r "echo hash_file('sha384', 'composer-setup.php');")"

if [ "$EXPECTED_SIGNATURE" != "$ACTUAL_SIGNATURE" ]
then
    >&2 echo 'ERROR: Invalid installer signature'
    rm composer-setup.php
    exit 1
fi

php composer-setup.php --quiet
RESULT=$?
rm composer-setup.php
exit $RESULT

Execute o script e instale os pacotes:

cd /var/www/html/syspass/
sh install-composer.sh
php composer.phar install --no-dev


---

Passo 7: Configurar o Apache

Crie o arquivo de configuração syspass.conf:

vim /etc/apache2/sites-available/syspass.conf

Adicione a seguinte configuração:

<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot "/var/www/html/syspass"
    ServerName syspass.example.com
    <Directory "/var/www/html/syspass/">
        Options MultiViews FollowSymlinks
        AllowOverride All
        Require all granted
    </Directory>
    TransferLog /var/log/apache2/syspass_access.log
    ErrorLog /var/log/apache2/syspass_error.log
</VirtualHost>

Para ativar o HTTPS, adicione:

<VirtualHost *:443>
    ServerAdmin admin@example.com
    DocumentRoot "/var/www/html/syspass"
    ServerName syspass.example.com
    SSLEngine on
    SSLCertificateFile /etc/apache2/certificate/syspass.crt
    SSLCertificateKeyFile /etc/apache2/certificate/syspass.key
    <Directory "/var/www/html/syspass/">
        Options MultiViews FollowSymlinks
        AllowOverride All
        Require all granted
    </Directory>
    TransferLog /var/log/apache2/syspass_access.log
    ErrorLog /var/log/apache2/syspass_error.log
</VirtualHost>

Ative o site e os módulos necessários:

a2ensite syspass
a2enmod rewrite ssl
systemctl reload apache2


---

Passo 8: Testar a instalação

Acesse o sysPass no navegador usando o domínio configurado, por exemplo: http://syspass.domonio.name

