# Fazendo Requisição POST com Postman na API do SYSPASS

Base: URL para acessar a API:

    $ https://server_name/api.php

---

1.0 Acesse o Syspass via WEB e crie um novo token de API AUTHORIZATION

    # https://syspass.vigilante.kira.br
    # Users and Accesses
    # API AUTHORIZATIONS
    # Botão + : New Authorization

1.1 Para cada função ou Action ( ação ) será necessário criar uma api unica.
exemplo:
- para pesquisar detalhes de uma conta, será uma action: account/search.
- para criar uma category, será uma acction: category/create.
  
        # User: Selecione o usuario dono da autorização
        # Action: Defina qual ação essa api poderá fazer:
        # Password: Defina a senha para a autorização: é a TokenPass.

- salve e clique em View Authorization: o contéudo do Token será usado no authToken.

        # Token: 210309sdsajdbnh12034231049234

  ---

  # Criando uma Requisição account/create

2.1 Dentro do Postman, coloque o metodo POST e a URL da API:

    # POST  |  https://syspass.vigilante.kira.br/api.php = exemplo.
    $ https://syspass.vigilante.kira.br/api.php = a url é de acordo com seu domain name.

2.2 Vá em Body, em seguida em raw e coloque o contéudo JSON-RPC:

- Exemplo Explicativo:

        {
          "jsonrpc": "2.0",
          "method": "account/create",
          "params": {
            "authToken": "3c51052421f35cea141d4778cfb663d4b1cfce21d0652403d970fa6673b1265c",
            "tokenPass": "12345678",
            "name": "Deadpool",
            "categoryId": "3",
            "clientId": "1",
            "pass": "123456"
        },
        "id": 1
        }

  - method = metodo de action que api vai utilizar.
  - authToken = tokenGerado após a criação da autorização.
  - tokenPass = é a senha utilizada na criação da autorização.
  - categoryId = é o ID da categoria existente a que foi criada - no caso de criar, vai ser uma ID livre disponível para ser usada.
  - clientID = é o ID do cliente correspondente; exemplo: client -> Conta API

