# Configuração SSL Auto Assinado para Syspass

1.0. Instale o Openssl:

    $ sudo apt install openssl

1.1. Ative o modo ssl e rewrite:

    $ a2enmod ssl
    $ a2enmod rewrite

---

# Gerando a chave e o certificado

2.0. Crie o diretorio do certificado:

    $ mkdir /etc/apache2/certificate

2.1. Mova para o diretorio:

    $ cd /etc/apache2/certificate

2.2. Gere uma chave privada e o certificado auto assinado do site:
    - commom_name é onde digita o ip ou o nome do host 
    
    $ openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out syspass.crt -keyout syspass.key

2.3. Certifique-se que o certificado e a chave estão no diretorio correto:

    $ /etc/apache2/certificate

---

# Configuração Apache

2.4. Edite o arquivo de configuração do Apache:

    $ sudo vim /etc/apache2/sites-available/syspass.conf
    
2.5. Copie o conteúdo e edite colocando os caminhos para a chave e o certificado:

    <VirtualHost *:80>
        ServerName syspass.vigilante.kira.br
        Redirect permanent / https://syspass.vigilante.kira.br/
    </VirtualHost>

    <VirtualHost *:443>
        ServerAdmin admin@localhost
        DocumentRoot "/var/www/html/syspass"
        ServerName syspass.exemplo.br
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

- Foi colocado um redirecionamento da porta 80 (http) para a porta 443 (https)

---

# Ativação

3.1. Ative o syspass e reinicia o serviço apache

      $ a2ensite syspass
      $ systemctl restart apache2
      $ systemctl status apache2

# Teste

4.1. Acesse a URL do syspass com https e o dominio configurado:

    $ https://syspass.domain.name 














 
