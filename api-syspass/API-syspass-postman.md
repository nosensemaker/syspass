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
- exemplo: para pesquisar detalhes de uma conta, será uma action: account/search
- para criar uma category, será uma acction: category/create

        # User: Selecione o usuario dono da autorização
        # Action: Defina qual ação essa api poderá fazer:
        # 
        
