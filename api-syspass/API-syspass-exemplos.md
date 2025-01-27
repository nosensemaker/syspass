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

{
  "jsonrpc": "2.0",
# metodo que api vai utilizar
  "method": "account/create",
  "params": {
      # tokenGerado após a criação da autorização
    "authToken": "3c51052421f35cea141d4778cfb663d4b1cfce21d0652403d970fa6673b1265c",
      # é a senha utilizada na criação da autorização
    "tokenPass": "12345678",
    "name": "Deadpool",
      # é o ID da categoria existinte! a que foi criada.
    "categoryId": "3",
      # é o ID do cliente correspondente criado 
    "clientId": "1",
    "pass": "123456"
    },
    "id": 1
}
