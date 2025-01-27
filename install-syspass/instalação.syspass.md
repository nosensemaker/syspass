# Instalação do sysPass no Ubuntu 22.4

1.0. Instale o servidor web Apache2 e o banco de dados MariaDB:

    $ sudo apt-get install apache2 mariadb-server -y

1.1. Adicione o repositoŕio para o PHP:

    $ sudo add-apt-repository ppa:ondrej/php -y

1.1. Instale pacotes adicionais:

    $ sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
    
1.2. Instale o PHP 7.4 e as extensões necessárias para o sysPass:

    $ sudo apt install libapache2-mod-php7.4 php7.4 php7.4-mysqli php7.4-pdo php7.4-cgi php7.4-cli php7.4-common php7.4-gd php7.4-json php7.4-readline php7.4-curl php7.4-intl php7.4-ldap php7.4-xml php7.4-mbstring git -y

1.3. Edite o arquivo de configuração do PHP:

    $ sudo vim /etc/php/7.4/apache2/php.ini
    
1.3.1 Faça os ajustes necessários:

     # post_max_size = 100M
     # upload_max_filesize = 100M
     # max_execution_time = 7200
     # memory_limit = 512M
     # date.timezone = UTC

1.4. Reinicie o Apache para aplicar as mudanças:

    $ sudo systemctl restart apache2

# Configure o banco de dados MariaDB

2.0. Execute o script de configuração do MariaDB:

    $ sudo mysql_secure_installation

2.0.1 Sugestão de Configuração:

    # Current password for root ( coloque a senha do root )
    # Unix_socket authentication = n
    # Change the root password? = n
    # Remove anonymous users? = y
    # Disallow root login remotely? = y
    # Remove test database and access to it? = y
    # Reload privilege tables now? = y
    

2.1. Acesse o MariaDB:

    $ sudo mysql -u root -p

2.2. Crie o banco de dados e configure o usuário de serviço:

    $ create database syspassdb;
    $ grant all privileges on syspassdb.* to 'syspassuser'@'localhost' identified by 'password'; 
    $ flush privileges;
    $ exit;
    
2.3 Crie usuario de administrador:

    $ sudo mysql -u root -p
    $ create user 'name'@'%' identified by 'password';
    $ grant all privileges on syspassdb.* to 'name'@'%';
    $ flush privileges;
    $ exit;

# Clone o repositório do sysPass

3.0. Clone o repositório oficial do GitHub:

    $ git clone https://github.com/nuxsmin/sysPass.git

3.1. Mova para o Direito do /var/www/html/syspass 

    $ sudo mv sysPass /var/www/html/syspass

3.2. Ajuste as permissões e o proprietário do diretório:

    $ sudo chown -R www-data:www-data /var/www/html/syspass 
    $ sudo chmod 750 /var/www/html/syspass/app/{config,backup}

# Instale o Composer:

4.0. Crie o script install-composer.sh no diretório do sysPass: 

    $ sudo vim /var/www/html/syspass/install-composer.sh

4.1. Adicione o seguinte conteúdo:


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



4.2.0. Mova para o diretorio do syspass e execute o script:

    $ cd /var/www/html/syspass/ 
    $ sudo sh install-composer.sh

4.3.0. Instale o Composer:

- Caso necessario force executar com o root com yes

        $ sudo php composer.phar install --no-dev

# Configure o Apache
5.0. Crie o arquivo de configuração syspass.conf:

    $ sudo vim /etc/apache2/sites-available/syspass.conf

5.1.0. Adicione a seguinte configuração:

    <VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot "/var/www/html/syspass"
    ServerName syspass.example.com
    <Directory "/var/www/html/syspass/">
    Options MultiViews FollowSymlinks
    AllowOverride All
    Order allow,deny
    Allow from all
    </Directory>
    TransferLog /var/log/apache2/syspass_access.log
    ErrorLog /var/log/apache2/syspass_error.log
    </VirtualHost>


5.1.2. Mude o ServerName para o nome do seu servidor, ex: syspass.vigilante.kira.br.

# Ativação

5.2. Ative o site e os módulos necessários: 

    $ sudo a2ensite syspass
    $ sudo systemctl reload apache2

6.0. Teste no navegador usando o dominio configurado.
- não esqueça de colocar no hosts da maquina local ou no dns o domain name configurado.

    exemplo: http://syspass.dominio.name $ http://syspass.domonio.name
