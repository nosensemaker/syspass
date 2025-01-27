# Instalação do sysPass no Ubuntu 22.4

1.0. Instale o servidor web Apache2 e o banco de dados MariaDB:

$ apt-get install apache2 mariadb-server -y

1.1. Instale pacotes adicionais e adicione o repositório para o PHP:
$ apt install software-properties-common ca-certificates lsb-release apt-transport-https -y add-apt-repository ppa:ondrej/php -y

1.2. Instale o PHP 7.4 e as extensões necessárias para o sysPass:
$ apt install libapache2-mod-php7.4 php7.4 php7.4-mysqli php7.4-pdo php7.4-cgi php7.4-cli php7.4-common php7.4-gd php7.4-json php7.4-readline php7.4-curl php7.4-intl php7.4-ldap php7.4-xml php7.4-mbstring git -y

1.3. Edite o arquivo de configuração do PHP:
$ vim /etc/php/7.4/apache2/php.ini

1.4. Reinicie o Apache para aplicar as mudanças:
$ systemctl restart apache2

# Configure o banco de dados MariaDB

2.0 Execute o script de configuração do MariaDB:
$ mysql_secure_installation

2.1. Acesse o MariaDB:
$ mysql -u root -p

2.2. Crie o banco de dados e configure o usuário: $ create database syspassdb;
$ grant all privileges on syspassdb.* to 'syspassuser'@'localhost' identified by 'password'; $ flush privileges; $ exit;
Clone o repositório do sysPass

3.0 Clone o repositório oficial do GitHub:
$ git clone https://github.com/nuxsmin/sysPass.git

3.1. Mova para o Direito do /var/www/html/syspass 
$ mv sysPass /var/www/html/syspass

3.2. Ajuste as permissões e o proprietário do diretório:
$ chown -R www-data:www-data /var/www/html/syspass 
$ chmod 750 /var/www/html/syspass/app/{config,backup}

# Instale o Composer:

4.0. Crie o script install-composer.sh no diretório do sysPass: 
$ vim /var/www/html/syspass/install-composer.sh

4.1.0 Adicione o seguinte conteúdo:

[script](.../nosensemaker/syspass/blob/main/install-composer.script)


4.2.0 Mova para o diretorio do syspass e execute o script:
$ cd /var/www/html/syspass/ $ sh install-composer.sh

4.3.0 Instale o Composer:
$ php composer.phar install --no-dev

# Configure o Apache
5.0. Crie o arquivo de configuração syspass.conf:
$ vim /etc/apache2/sites-available/syspass.conf

5.1.0 Adicione a seguinte configuração:

<VirtualHost *:80> ServerAdmin admin@localhost
DocumentRoot "/var/www/html/syspass" ServerName syspass.example.com <Directory "/var/www/html/syspass/"> Options MultiViews FollowSymlinks AllowOverride All Require all granted TransferLog /var/log/apache2/syspass_access.log ErrorLog /var/log/apache2/syspass_error.log

5.1.0: Mude o ServerName para o nome do seu servidor, ex: syspass.vigilante.kira.br.

# Ativação

5.2.0 Ative o site e os módulos necessários: 
$ a2ensite syspass $ a2enmod rewrite ssl $ systemctl reload apache2

6.0.0 Teste no navegador usando o dominio configurado.

    exemplo: http://syspass.dominio.name $ http://syspass.domonio.name
