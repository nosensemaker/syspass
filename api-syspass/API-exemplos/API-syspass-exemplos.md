# API SYSPASS EXEMPLOS DE USOS

- URL para exemplos e mais informações da API: https://syspass-doc.readthedocs.io/en/3.0/application/api.html
- URL para Acessar a API:   $ https://syspass.domain.name/api.php
---

- exemplo: account/search =  para procurar account

      {
        "jsonrpc": "2.0",
        "method": "account/search",
        "params": {
        "authToken": "auth_token_for_api"
      },
      "id": 1
      }


- exemplo: account/create: criar account.

      {
        "jsonrpc": "2.0",
        "method": "account/create",
        "params": {
          "authToken": "token_api",
          "tokenPass": "senha_api",
          "name": "name",
          "categoryId": "3",
          "clientId": "1",
          "pass": "senha"
          },
          "id": 1
        {

- exemplo: category/create: criar categoria nova.

      {
        "jsonrpc": "2.0",
        "method": "category/create",
        "params": {
          "authToken": "token_api",
          "tokenPass": "senha_api",
          "name": "name",
          "description": "Teste final do teste"
          },
          "id": 1
      }


- method = metodo de action que api vai utilizar.
- authToken = tokenGerado após a criação da autorização.
- tokenPass = é a senha utilizada na criação da autorização.
- categoryId = é o ID da categoria existente a que foi criada - no caso de criar, vai ser uma ID livre disponível para ser usada.
- clientID = é o ID do cliente correspondente; exemplo: client -> Conta API
