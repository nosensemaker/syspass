# API SYSPASS EXEMPLOS DE USOS

- URL para exemplos da API: https://syspass-doc.readthedocs.io/en/3.0/application/api.html
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


- exemplo: account/create

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
        {

- exemplo: category/create

      {
        "jsonrpc": "2.0",
        "method": "category/create",
        "params": {
          "authToken": "3c51052421f35cea141d4778cfb663d4b1cfce21d0652403d970fa6673b1265c",
          "tokenPass": "12345678",
          "name": "marcelo",
          "description": "Teste final do teste"
          },
          "id": 1
      }
