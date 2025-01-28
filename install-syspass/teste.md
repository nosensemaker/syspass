# Instalação do sysPass no Ubuntu 22.04

## 1. Instalar o servidor web Apache2 e o banco de dados MariaDB

1.1. Instalar os pacotes necessários:

```bash
sudo apt-get install apache2 mariadb-server -y
```

1.2. Adicionar o repositório para o PHP:

```bash
sudo add-apt-repository ppa:ondrej/php -y
```

1.3. Instalar pacotes adicionais:

```bash
sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
```

1.4. Instalar o PHP 7.4 e as extensões necessárias:

```bash
sudo apt install libapache2-mod-php7.4 php7.4 php7.4-mysqli php7.4-pdo php7.4-cgi php7.4-cli php7.4-common php7.4-gd php7.4-json php7.4-readline php7.4-curl php7.4-intl php7.4-ldap php7.4-xml php7.4-mbstring git -y
```

1.5. Editar o arquivo de configuração do PHP:

```bash
sudo vim /etc/php/7.4/apache2/php.ini
```

1.6. Ajustar as configurações necessárias:

```
post_max_size = 100M
upload_max_filesize = 100M
max_execution_time = 7200
memory_limit = 512M
date.timezone = UTC
```

1.7. Reiniciar o Apache para aplicar as mudanças:

```bash
sudo systemctl restart apache2
```

---

## 2. Configurar o banco de dados MariaDB

2.1. Executar o script de configuração inicial do MariaDB:

```bash
sudo mysql_secure_installation
```

### Sugestão de configuração:

- Current password for root: insira a senha do root
- Unix_socket authentication: `n`
- Change the root password? `n`
- Remove anonymous users? `y`
- Disallow root login remotely? `y`
- Remove test database and access to it? `y`
- Reload privilege tables now? `y`

2.2. Acessar o MariaDB:

```bash
sudo mysql -u root -p
```

2.3. Criar o banco de dados e configurar o usuário de serviço:

```sql
CREATE DATABASE syspassdb;
GRANT ALL PRIVILEGES ON syspassdb.* TO 'syspassuser'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
EXIT;
```

2.4. Criar um usuário administrador adicional:

```bash
sudo mysql -u root -p
```

```sql
CREATE USER 'name'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON syspassdb.* TO 'name'@'%';
FLUSH PRIVILEGES;
EXIT;
```

---

## 3. Clonar o repositório do sysPass

3.1. Clonar o repositório oficial do GitHub:

```bash
git clone https://github.com/nuxsmin/sysPass.git
```

3.2. Mover o diretório para `/var/www/html/syspass`:

```bash
sudo mv sysPass /var/www/html/syspass
```

3.3. Ajustar as permissões e o proprietário do diretório:

```bash
sudo chown -R www-data:www-data /var/www/html/syspass
sudo chmod 750 /var/www/html/syspass/app/{config,backup}
```

---

## 4. Instalar o Composer

4.1. Criar o script `install-composer.sh` no diretório do sysPass:

```bash
sudo vim /var/www/html/syspass/install-composer.sh
```

4.2. Adicionar o seguinte conteúdo:

```bash
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
```

4.3. Mover para o diretório do sysPass e executar o script:

```bash
cd /var/www/html/syspass/
sudo sh install-composer.sh
```

4.4. Instalar o Composer:

```bash
sudo php composer.phar install --no-dev
```

*Nota: Caso necessário, force a execução com `yes`.*

---

## 5. Configurar o Apache

5.1. Criar o arquivo de configuração `syspass.conf`:

```bash
sudo vim /etc/apache2/sites-available/syspass.conf
```

5.2. Adicionar a seguinte configuração:

```apache
<VirtualHost *:80>
    ServerAdmin admin@localhost
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
```

5.3. Alterar o `ServerName` para o nome do seu servidor (exemplo: `syspass.vigilante.kira.br`).

5.4. Ativar o site e os módulos necessários:

```bash
sudo a2ensite syspass
sudo systemctl reload apache2
```

---

## 6. Testar a configuração

6.1. Testar no navegador usando o domínio configurado.

*Exemplo:*

- `http://syspass.dominio.name`

**Nota:** Não se esqueça de adicionar o domínio configurado no arquivo `hosts` da máquina local ou no DNS.

