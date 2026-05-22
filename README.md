# Contrato API Design - Sistema de Fast Food

Este repositório contém a especificação técnica da API para gerenciamento do catálogo e fluxo de pedidos de um sistema de Fast Food. O contrato foi desenhado utilizando o padrão **OpenAPI 3.0.0**, garantindo uma estrutura padronizada, de fácil integração e pronta para geração de ferramentas automatizadas (como mocks, SDKs e documentações interativas via Swagger UI ou Redoc).

## 👥 Integrantes do Grupo

* **Felipe Bergamin Dantas** - RA: 103538
* **Rafael Alves Oliveira** - RA: 76601
* **Paulo César Bezerra de Lima Silva** - RA: 76348
* **Lucas Sena Barbosa** - RA: 75806
* **Luis Henrique Marques** - RA: 77215

---

## 🚀 Informações Gerais do Servidor

* **Título da API:** `contrato_api_design`
* **Versão do Contrato:** `v1`
* **Ambiente de Produção:** `https://api.contrato_api_design.com/v1`
* **Formato de Dados:** JSON (`application/json`)

---

## 🔒 Segurança e Autenticação

A API utiliza o protocolo de segurança **OAuth2** com o fluxo de **Client Credentials** (`clientCredentials`). 
* **URL de Token:** `https://api.contrato_api_design.com/oauth/token`
* **Aplicações:** A maioria dos endpoints exige o cabeçalho de autorização (Bearer token) obtido através deste fluxo.

---

## 🗺️ Arquitetura de Endpoints (Paths)

A API é dividida logicamente em três escopos principais: **Produtos**, **Pedidos** e **Usuários**.

### 🍔 1. Módulo de Produtos (`/produtos`)
Gerencia o catálogo de itens comercializados pelo fast food (lanches, acompanhamentos, bebidas, etc.).

* `GET /produtos`: Lista todos os produtos cadastrados com suporte a paginação e filtros.
    * **Query Params:** `busca` (por nome), `categoria`, `ordem` (`asc` ou `desc`), `page` (padrão: 1) e `per_page` (padrão: 15).
    * **Respostas:** `200` (Retorna os dados paginados e um array de produtos), `400` (Dados inválidos), `401` (Não autorizado).
* `POST /produtos`: Registra um novo produto no sistema.
    * **Body:** Objeto do tipo `ProdutoInput` contendo os campos obrigatórios (`nome`, `descricao`, `preco`) e opcional (`categoria_id`).
* `GET /produtos/{id}`: Detalha um produto específico pelo seu ID numérico.
* `PUT /produtos/{id}`: Atualiza completamente os dados de um produto existente.
* `DELETE /produtos/{id}`: Remove permanentemente um produto do catálogo.

### 📝 2. Módulo de Pedidos (`/pedidos`)
Orquestra a criação e fluxo de requisições de compras dos clientes.

* `POST /pedidos`: Cria um novo pedido de Fast Food.
    * **Body:** Recebe uma lista (`array`) com os IDs numéricos dos produtos desejados e uma string de `observacao`.
    * **Resposta:** `201` com o payload do pedido gerado, cálculo automático do preço total e status padrão inicial como `"pendente"`.
* `GET /pedidos/{id}`: Consulta os detalhes e o status atualizado de um pedido específico.

### 👥 3. Módulo de Usuários (`/users`)
Controla o cadastro dos perfis de usuários no sistema.

* `GET /users`: Retorna a listagem completa dos usuários cadastrados na plataforma.
* `PATCH /users/{id}`: Permite a modificação parcial ou pontual das informações do usuário (como alterar somente o `nome` ou somente o `email`).

---

## 📦 Modelos de Dados (Schemas)

A API possui estruturas de dados bem definidas para garantir a consistência das requisições e respostas:

1. **Produto:** Representação completa do item do catálogo. Possui propriedades como `id`, `nome`, `preco` (float), `descricao`, um objeto aninhado `categoria` (`id` e `nome`), lista de URIs de `imagens` e timestamp `criado_em`.
2. **ProdutoInput:** Payload reduzido para criação/edição de produtos. Exige as propriedades cruciais e substitui o objeto categoria por uma referência simples de ID (`categoria_id`).
3. **Pedido:** Estrutura de saída do pedido, contendo uma lista de objetos do tipo `Produto`, valor `total`, string de `status` e a data de criação.
4. **PedidoInput:** Payload de entrada do pedido, que simplifica a requisição recebendo apenas os IDs inteiros dos produtos escolhidos e a observação.
5. **User:** Contém dados de perfil como `id`, `nome`, `email` formatado e `criado_em`.
6. **UserUpdateInput:** Campos opcionais para modificações via método PATCH.

---

## 🛠️ Como utilizar este contrato

Para visualizar este contrato de forma gráfica ou gerar servidores e clientes mockados a partir dele:
1. Copie o código YAML fornecido no seu projeto.
2. Acesse o [Swagger Editor](https://editor.swagger.io/) no seu navegador.
3. Cole o conteúdo no painel esquerdo.
4. Utilize a interface interativa no painel direito para estudar a API ou exportar os artefatos de código necessários.
