# Instalando o Postman para requisição de API

1.0. Certifique-se que o curl e o snap estejam instalados:
        
        $ sudo apt update
        $ sudo apt install curl -y
        $ sudo apt install snapd -y

# Instalação do Postman via SNAP

2.0. Instele o Postman usando o snap

        $ sudo snap install postman

2.1. Verifique se o Postman foi instaldo.

        $ snap list | grep postman

2.2. Abra o postman pelo terminal ou manualmente pesquisando pelo nome:

        $ postman

# Instalação do Postman manualmente

2.0.1. Acesse o site do Postman oficial:

        $ https://www.postman.com/downloads/

2.0.2. Faça o Download na versao linux(x64)

2.0.3. Extraia o arquivo postman-linux-x64.tar.gz:

2.0.4. Entre no diretorio que foi extraído pela interface e procure pelo Postman executavel para abrir.

# Primeiros passos:

3.0. Crie sua conta ou continue sem conta; preferível criar a conta.

3.1.0 Crie uma Request:

        # NEW -> HTTP

3.2.0 Troque o metodo GET pelo POST: 

        # POST =  Usado para enviar dados ao servidor. Utilizado para criar novos recursos em uma API.
        # GET  =  Serve para obter recursos ou dados sem modificar o estado do servidor.

3.3.0 Coloque os dados no Body do POST:

        # Os dados enviados no método Post devem ser incluídos no body
        $ Body > raw # coloque nesse espaço

