
# Exemplos de Uso da API do sysPass

- URL para exemplos e mais informações da API: [Documentação oficial](https://syspass-doc.readthedocs.io/en/3.0/application/api.html)  
- URL para acessar a API: `https://syspass.domain.name/api.php`

---

## Exemplos de Requisições

### **1. Procurar conta (account/search)**

#### Exemplo de Payload
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

#### Requisitos
- Criar uma conta no sysPass previamente.

---

### **2. Criar conta (account/create)**

#### Exemplo de Payload
```json
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
}
```

#### Requisitos
- Criar uma categoria, uma conta e um cliente no sistema previamente.

---

### **3. Criar categoria (category/create)**

#### Exemplo de Payload
```json
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
```

#### Requisitos
- Configurar autenticação válida para utilizar a API.

---

## Parâmetros Importantes

- **`method`**: Método de ação que a API irá utilizar.
- **`authToken`**: Token gerado após a criação da autorização.
- **`tokenPass`**: Senha utilizada na criação da autorização.
- **`categoryId`**: ID da categoria existente ou criada. Para criar uma nova, será gerada uma ID disponível.
- **`clientId`**: ID do cliente correspondente. Exemplo: cliente vinculado a uma conta da API.
