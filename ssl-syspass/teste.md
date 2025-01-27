# Configuração SSL Auto Assinado para Syspass

## 1. Instalar o OpenSSL

```bash
sudo apt install openssl
```

---

## 2. Gerar a chave e o certificado

### 2.1 Criar o diretório do certificado:

```bash
sudo mkdir /etc/apache2/certificate
```

### 2.2 Acessar o diretório criado:

```bash
cd /etc/apache2/certificate
```

### 2.3 Gerar a chave privada e o certificado autoassinado:

```bash
sudo openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out syspass.crt -keyout syspass.key
```

#### Preencher as informações com os seguintes valores:

- **Country Name:** BR
- **State or Province Name:** Distrito Federal
- **Locality Name:** Brasília
- **Organization Name:** ITI - Instituto Nacional de Tecnologia da Informação
- **Organization Unit:** ICP-Brasil
- **Common Name:** Insira o nome do domínio configurado, ex: `syspass.kira.br`.
- **Email Address:** Opcional.

> **Dica:** Certificar-se de que o valor de **Common Name** corresponde ao domínio usado no servidor.

### 2.4 Verificar se o certificado e a chave estão no diretório correto:

```bash
sudo ls /etc/apache2/certificate
```

---

## 3. Configurar o Apache

### 3.1 Editar o arquivo de configuração do Apache:

```bash
sudo vim /etc/apache2/sites-available/syspass.conf
```

### 3.2 Substituir o conteúdo pelo seguinte:

```apache
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
```

#### Explicação:

- **Redirecionar HTTP para HTTPS:** A primeira seção do VirtualHost garante que o tráfego na porta 80 seja redirecionado para a porta 443.
- **Caminhos do certificado:** Ajustar os valores de `SSLCertificateFile` e `SSLCertificateKeyFile` para os arquivos gerados.
- **DocumentRoot:** Definir o diretório onde estão os arquivos do Syspass.
- **ServerAdmin:"" Localhost é definido em servidores de teste ou desenvolvimento.

---

## 4. Ativar a configuração e reiniciar o Apache

### 4.1 Ativar os módulos SSL e Rewrite:

```bash
a2enmod ssl
a2enmod rewrite
```

### 4.2 Habilitar o site configurado:

```bash
sudo a2ensite syspass
```

### 4.3 Reiniciar o serviço do Apache:

```bash
sudo systemctl restart apache2
```

### 4.4 Verificar o status do Apache:

```bash
sudo systemctl status apache2
```

> **Nota:** Certificar-se de que o status indica que o serviço está ativo e rodando sem erros.

---

## 5. Acessar a URL configurada

### 5.1 Abrir o navegador e inserir o domínio configurado com HTTPS:

```text
https://syspass.domain.name
```

> **Importante:** Certificar-se de usar o domínio configurado na configuração do Apache e coloca-lo no hosts ou no DNS da sua máquina local.
