# Instalando o Postman para Requisição de API

## 1. Preparar o Ambiente

Certifique-se de que o `curl` e o `snap` estejam instalados:

```bash
sudo apt update
sudo apt install curl -y
sudo apt install snapd -y
```

---

## 2. Instalação do Postman via Snap

2.1. Instalar o Postman usando o Snap:

```bash
sudo snap install postman
```

2.2. Verificar se o Postman foi instalado corretamente:

```bash
snap list | grep postman
```

2.3. Abrir o Postman pelo terminal ou manualmente:

```bash
postman
```

---

## 3. Instalação do Postman Manualmente

3.1. Acessar o site oficial do Postman:

[https://www.postman.com/downloads/](https://www.postman.com/downloads/)

3.2. Fazer o download da versão para Linux (x64).

3.3. Extrair o arquivo `postman-linux-x64.tar.gz` na pasta de downloads:

```bash
cd /home/user/Downloads
tar -xvzf postman-linux-x64.tar.gz
```

3.4. Navegar até o diretório extraído e executar o Postman:

```bash
cd postman
./Postman
```

---

## 4. Primeiros Passos

4.1. Criar uma conta ou continuar sem conta (preferível criar uma conta).

4.2. Criar uma nova requisição:

1. Clique em **New**.
2. Escolha o método HTTP.

4.3. Alterar o método de **GET** para **POST**:

- **POST**: Usado para enviar dados ao servidor. Utilizado para criar novos recursos em uma API.
- **GET**: Serve para obter recursos ou dados sem modificar o estado do servidor.

4.4. Inserir dados no corpo da requisição **POST**:

1. Navegue até a aba **Body**.
2. Escolha **raw**.
3. Insira os dados no espaço designado.

---

