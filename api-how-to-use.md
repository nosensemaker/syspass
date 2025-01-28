# Fazer Requisição POST com Postman na API do sysPass

## Base URL para acessar a API

```bash
https://server_name/api.php
```

---

## 1. Criar um Novo Token de Autorização

1.1. Acessar o sysPass via web:

```bash
https://syspass.vigilante.kira.br
```

1.2. Navegar até **Users and Accesses** > **API Authorizations**.

1.3. Clicar no botão "+" e selecione **New Authorization**.

1.4. Preencher os campos:

- **User**: Selecione o usuário dono da autorização.
- **Action**: Defina ações que essa API poderá realizar (ex.: `account/search`, `category/create`, etc.).
- **Password**: Defina uma senha para a autorização (TokenPass).

1.5. Salvar a autorização e clique em **View Authorization** para obter o token gerado.

Exemplo de Token:

```bash
Token: 210309sdsajdbnh12034231049234
```

---

## 2. Criar uma Account no sysPass: manualmente.

2.1. Clique em **New Account** no menu.

2.3. **Preencher os detalhes da Account**  

   - **Name**: Insira o nome da Account.  
   - **Client**: Selecione o cliente correspondente.  
     - Caso não exista, clique no botão **+**.  
     - Uma aba chamada **New Client** será aberta.  
       - Preencha o nome do cliente e uma descrição.  

   - **Category**: Selecione a categoria correspondente.  
     - Caso não exista, clique no botão **+**.  
     - Uma aba chamada **New Category** será aberta.  
       - Preencha o nome da categoria e uma descrição.  

   - **User**: Insira o usuário responsável pela Account.  

   - **Password**: Insira a senha que será salva para esta Account.  

3. **Preencher informações adicionais (opcional)**  
   - Insira quaisquer outras informações nos campos disponíveis, conforme necessário.

4. **Salvar a Account**  
   - Clique em **Save** para concluir o processo de criação da Account.
   
---

## 3. Criando uma Requisição `account/search`

### Requisitos:
- Ter uma conta já salva no sysPass.

### Passos:

3.1. No Postman, configure o método como **POST** e insira a URL da API:

```bash
https://syspass.vigilante.kira.br/api.php
```

3.2. Navegue até a aba **Body**, selecione **raw**, e insira o conteúdo JSON-RPC:

```json
{
  "jsonrpc": "2.0",
  "method": "account/search",
  "params": {
    "authToken": "auth_token_for_api"
  },
  "id": 1
}
```

### Explicação dos campos:
- **method**: Define a ação da API.
- **authToken**: Token gerado após criar a autorização.

3.3. Clique em **Send**. A resposta será semelhante a:

```json
{
    "jsonrpc": "2.0",
    "result": {
        "itemId": 0,
        "result": [
            {
                "id": 1,
                "name": "lethicia",
                "categoryName": "league of legends",
                "clientName": "league of legends",
                "userName": "sysPass Admin",
                "userLogin": "admin",
                "countView": 1
            }
        ],
        "resultCode": 0,
        "resultMessage": null
    },
    "id": 1
}
```

### Caso de Erro:
Se ocorrer um erro de SSL ("SELF-SIGNED CERTIFICATE"), desabilite a verificação SSL no Postman.

```bash
Disable SSL Verification
```

---

## 4. Criando uma Requisição `account/create`

### Requisitos:
- Ter uma categoria e um cliente criados no sysPass.

### Passos:

4.1. No Postman, configure o método como **POST** e insira a URL da API:

```bash
https://syspass.vigilante.kira.br/api.php
```

4.2. Navegue até a aba **Body**, selecione **raw**, e insira o conteúdo JSON-RPC:

```json
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
```

### Explicação dos campos:
- **method**: Define a ação da API.
- **authToken**: Token gerado após criar a autorização.
- **tokenPass**: Senha utilizada na criação da autorização.
- **categoryId**: ID da categoria existente.
- **clientId**: ID do cliente correspondente.

4.3. Clique em **Send**. A resposta será semelhante a:

```json
{
    "jsonrpc": "2.0",
    "result": {
        "itemId": 17,
        "result": {
            "id": 17,
            "name": "Deadpool",
            "categoryName": "API",
            "clientName": "Contas API",
            "userName": "sysPass Admin",
            "dateAdd": "2025-01-27 14:08:52"
        },
        "resultCode": 0,
        "resultMessage": "Account created"
    },
    "id": 1
}
```

