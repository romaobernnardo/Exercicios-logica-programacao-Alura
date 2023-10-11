# Projeto Sistema Financeiro

Este é um projeto de uma API REST para um sistema bancário desenvolvido em JavaScript usando o Express.js. Ele permite listar contas bancárias, criar novas contas, atualizar informações de contas existentes, deletar contas e realizar operações bancárias como depósitos, saques e transferências.

## Funcionalidades

- Cadastrar Usuário
- Fazer Login
- Detalhar Perfil do Usuário Logado
- Editar Perfil do Usuário Logado
- Listar categorias
- Listar transações
- Detalhar transação
- Cadastrar transação
- Editar transação
- Remover transação
- Obter extrato de transações
- Filtrar transações por categoria

## Estrutura do Código

O código-fonte do projeto está organizado da seguinte maneira:

<img src="./src/images/imagem-estrutura.png" alt="Estrutura do Projeto" width="250">

- `index.js`: O ponto de entrada do servidor Express.
- `rotas.js`: Define as rotas da API e seus controladores.
- `bancodedados.js`: Armazena os dados do banco e as funcionalidades relacionadas ao banco.

## **Banco de dados**

Foi criado um Banco de Dados PostgreSQL chamado `dindin` contendo as seguintes tabelas e colunas:  

- usuarios
  - id
  - nome
  - email (campo único)
  - senha
- categorias
  - id
  - descricao
- transacoes
  - id
  - descricao
  - valor
  - data
  - categoria_id
  - usuario_id
  - tipo

**Todos os dados de tabelas do banco de dados está no arquivo dump.sql**

Inserida as seguintes categorias que foram cadastradas para que sejam listadas no endpoint de listagem das categorias futuramente.

## **Categorias**

- Alimentação
- Assinaturas e Serviços
- Casa
- Mercado
- Cuidados Pessoais
- Educação
- Família
- Lazer
- Pets
- Presentes
- Roupas
- Saúde
- Transporte
- Salário
- Vendas
- Outras receitas
- Outras despesas

## **Endpoints**

### **Cadastrar usuário**

#### `POST` `/usuario`

Essa é a rota para cadastrar um novo usuario no sistema.

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  O corpo (body) possui um objeto com as seguintes propriedades:

  - nome
  - email
  - senha

- **Resposta**  
  Em caso de **sucesso**, é enviado no corpo (body) da resposta o conteúdo do usuário cadastrado, incluindo seu respectivo `id` e exceto a senha criptografada.
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// POST /usuario
{
    "nome": "José",
    "email": "jose@email.com",
    "senha": "123456"
}
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
{
    "id": 1,
    "nome": "José",
    "email": "jose@email.com"
}
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Já existe usuário cadastrado com o e-mail informado."
}
```

### **Login do usuário**

#### `POST` `/login`

Essa é a rota que permite o usuario cadastrado realizar o login no sistema.

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  O corpo (body) possui um objeto com as seguintes propriedades (respeitando estes nomes):

  - email
  - senha

- **Resposta**  
  Em caso de **sucesso**, o corpo (body) da resposta apresenta um objeto com a propriedade **token** que possui como valor o token de autenticação gerado e uma propriedade **usuario** que possui as informações do usuário autenticado, exceto a senha do usuário.
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.


#### **Exemplo de requisição**

```javascript
// POST /login
{
    "email": "jose@email.com",
    "senha": "123456"
}
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
{
    "usuario": {
        "id": 1,
        "nome": "José",
        "email": "jose@email.com"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiaWF0IjoxNjIzMjQ5NjIxLCJleHAiOjE2MjMyNzg0MjF9.KLR9t7m_JQJfpuRv9_8H2-XJ92TSjKhGPxJXVfX6wBI"
}
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Usuário e/ou senha inválido(s)."
}
```

---

### **Detalhar usuário**

#### `GET` `/usuario`

Essa é a rota que será chamada quando o usuario quiser obter os dados do seu próprio perfil.  

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  Não possui conteúdo no corpo da requisição.

- **Resposta**  
  Em caso de **sucesso**, o corpo (body) da resposta possui um objeto que representa o usuário encontrado, com todas as suas propriedades (exceto a senha), conforme exemplo abaixo, acompanhado de **_status code_** apropriado.  
 Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// GET /usuario
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
{
    "id": 1,
    "nome": "José",
    "email": "jose@email.com"
}
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Para acessar este recurso um token de autenticação válido deve ser enviado."
}
```

### **Atualizar usuário**

#### `PUT` `/usuario`

Essa é a rota que será chamada quando o usuário quiser realizar alterações no seu próprio usuário.  

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  O corpo (body) possui um objeto com as seguintes propriedades (respeitando estes nomes):

  - nome
  - email
  - senha

- **Resposta**  
    Em caso de **sucesso**, não é enviado conteúdo no corpo (body) da resposta.
    Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// PUT /usuario
{
    "nome": "José de Abreu",
    "email": "jose_abreu@email.com",
    "senha": "j4321"
}
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
// Sem conteúdo no corpo (body) da resposta
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "O e-mail informado já está sendo utilizado por outro usuário."
}
```

### **Listar categorias**

#### `GET` `/categoria`

Essa é a rota que será chamada quando o usuario logado quiser listar todas as categorias cadastradas.

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  Não possui conteúdo no corpo (body) da requisição.

- **Resposta**  
  Em caso de **sucesso**, o corpo (body) da resposta possui um array dos objetos (categorias) encontrados.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// GET /categoria
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
[
  {
    id: 1,
    descricao: "Roupas",
  },
  {
    id: 2,
    descricao: "Mercado",
  },
];
```

```javascript
// HTTP Status 200 / 201 / 204
[];
```

### **Listar transações do usuário logado**

#### `GET` `/transacao`

Essa é a rota que será chamada quando o usuario logado quiser listar todas as suas transações cadastradas.  

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  Não possui conteúdo no corpo (body) da requisição.

- **Resposta**  
  Em caso de **sucesso**, o corpo (body) da resposta possui um array dos objetos (transações) encontrados.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// GET /transacao
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
[
  {
    id: 1,
    tipo: "saida",
    descricao: "Sapato amarelo",
    valor: 15800,
    data: "2022-03-23T15:35:00.000Z",
    usuario_id: 5,
    categoria_id: 4,
    categoria_nome: "Roupas",
  },
  {
    id: 3,
    tipo: "entrada",
    descricao: "Salário",
    valor: 300000,
    data: "2022-03-24T15:30:00.000Z",
    usuario_id: 5,
    categoria_id: 6,
    categoria_nome: "Salários",
  },
];
```

```javascript
// HTTP Status 200 / 201 / 204
[];
```

### **Detalhar uma transação do usuário logado**

#### `GET` `/transacao/:id`

Essa é a rota que será chamada quando o usuario logado quiser obter uma das suas transações cadastradas.  

- **Requisição**  
  Enviado o ID da transação no parâmetro de rota do endpoint.  
  O corpo (body) da requisição não possui nenhum conteúdo.

- **Resposta**  
  Em caso de **sucesso**, o corpo (body) da resposta possui um objeto que representa a transação encontrada, com todas as suas propriedades, conforme exemplo abaixo, acompanhado de **_status code_** apropriado.  
   Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.


#### **Exemplo de requisição**

```javascript
// GET /transacao/2
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
{
    "id": 3,
    "tipo": "entrada",
    "descricao": "Salário",
    "valor": 300000,
    "data": "2022-03-24T15:30:00.000Z",
    "usuario_id": 5,
    "categoria_id": 6,
    "categoria_nome": "Salários",
}
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Transação não encontrada."
}
```

### **Cadastrar transação para o usuário logado**

#### `POST` `/transacao`

Essa é a rota que será utilizada para cadastrar uma transação associada ao usuário logado.  

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  O corpo (body) da requisição possui um objeto com as seguintes propriedades:

  - descricao
  - valor
  - data
  - categoria_id
  - tipo (campo que será informado se a transação corresponde a uma saída ou entrada de valores)

- **Resposta**
  Em caso de **sucesso**, é enviado, no corpo (body) da resposta, as informações da transação cadastrada, incluindo seu respectivo `id`.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// POST /transacao
{
    "tipo": "entrada",
    "descricao": "Salário",
    "valor": 300000,
    "data": "2022-03-24T15:30:00.000Z",
    "categoria_id": 6
}
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
{
    "id": 3,
    "tipo": "entrada",
    "descricao": "Salário",
    "valor": 300000,
    "data": "2022-03-24T15:30:00.000Z",
    "usuario_id": 5,
    "categoria_id": 6,
    "categoria_nome": "Salários",
}
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Todos os campos obrigatórios devem ser informados."
}
```

### **Atualizar transação do usuário logado**

#### `PUT` `/transacao/:id`

Essa é a rota que será chamada quando o usuario logado quiser atualizar uma das suas transações cadastradas.  

- **Requisição**  
  É enviado o ID da transação no parâmetro de rota do endpoint.  
  O corpo (body) da requisição possui um objeto com as seguintes propriedades (respeitando estes nomes):

  - descricao
  - valor
  - data
  - categoria_id
  - tipo (campo que será informado se a transação corresponde a uma saída ou entrada de valores)

- **Resposta**  
  Em caso de **sucesso**, não deveremos enviar conteúdo no corpo (body) da resposta.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// PUT /transacao/2
{
 "descricao": "Sapato amarelo",
 "valor": 15800,
 "data": "2022-03-23 12:35:00",
 "categoria_id": 4,
 "tipo": "saida"
}
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
// Sem conteúdo no corpo (body) da resposta
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Todos os campos obrigatórios devem ser informados."
}
```

### **Excluir transação do usuário logado**

#### `DELETE` `/transacao/:id`

Essa é a rota que será chamada quando o usuario logado quiser excluir uma das suas transações cadastradas.  

- **Requisição**  
  É enviado o ID da transação no parâmetro de rota do endpoint.  
  O corpo (body) da requisição não possui nenhum conteúdo.

- **Resposta**  
  Em caso de **sucesso**, não é enviado conteúdo no corpo (body) da resposta.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// DELETE /transacao/2
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
// Sem conteúdo no corpo (body) da resposta
```

```javascript
// HTTP Status 400 / 401 / 403 / 404
{
    "mensagem": "Transação não encontrada."
}
```

### **Obter extrato de transações**

#### `GET` `/transacao/extrato`

Essa é a rota que será chamada quando o usuario logado quiser obter o extrato de todas as suas transações cadastradas.

- **Requisição**  
  Sem parâmetros de rota ou de query.  
  O corpo (body) da requisição não possui nenhum conteúdo.

- **Resposta**  
  Em caso de **sucesso**, é enviado no corpo (body) da resposta um objeto contendo a soma de todas as transações do tipo `entrada` e a soma de todas as transações do tipo `saida`.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// DELETE /transacao/extrato
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
{
 "entrada": 300000,
 "saida": 15800
}
```

---

### **Filtrar transações por categoria**

Na funcionalidade de listagem de transações do usuário logado (**GET /transacao**), incluimos um parâmetro do tipo query **filtro** para que seja possível consultar apenas transações das categorias informadas.

- **Requisição**  
  Parâmetro opcional do tipo query **filtro**.
  Não possui conteúdo no corpo (body) da requisição.

- **Resposta**  
  Em caso de **sucesso**, o corpo (body) da resposta possui um array dos objetos (transações) encontradas.  
  Em caso de **falha na validação**, a resposta possui **_status code_** apropriado, e em seu corpo (body) possui um objeto com uma propriedade **mensagem** com valor de um texto explicando o motivo da falha.

#### **Exemplo de requisição**

```javascript
// GET /transacao?filtro[]=roupas&filtro[]=salários
// Sem conteúdo no corpo (body) da requisição
```

#### **Exemplos de resposta**

```javascript
// HTTP Status 200 / 201 / 204
[
  {
    id: 1,
    tipo: "saida",
    descricao: "Sapato amarelo",
    valor: 15800,
    data: "2022-03-23T15:35:00.000Z",
    usuario_id: 5,
    categoria_id: 4,
    categoria_nome: "Roupas",
  },
  {
    id: 3,
    tipo: "entrada",
    descricao: "Salário",
    valor: 300000,
    data: "2022-03-24T15:30:00.000Z",
    usuario_id: 5,
    categoria_id: 6,
    categoria_nome: "Salários",
  },
];
```

```javascript
// HTTP Status 200 / 201 / 204
[];
```

---


## Como Executar o Projeto

### Pré-requisitos

- Git instalado.
- Node.js instalado.
- Express.Js - framework instalado.
- node-postgres - biblioteca instalada.
- bcrypt - biblioteca instalada.
- jsonwebtoken - biblioteca instalada.
- Nodemon - utilitário instalado.
- Insomnia ou outra ferramenta similar para testar a API.

### Passos para Executar o Projeto

1. Para executar o projeto no seu ambiente de desenvolvimento em execução, primeiramente faça o clone desse repositório em sua maquina local na pasta desejada:

 ```
   git clone https://github.com/seu-usuario/nome-do-repositorio.git
```


1. Acesse a pasta do projeto
cd nome-do-repositorio

2. Depois abra o projeto em seu editor de códigos, abra o terminal e digite o seguinte comando para a instalação dos pacotes e dependências necessárias:

```
npm install
```
3. Execute a aplicação em modo de desenvolvimento:

```
npm run dev
```

4. Use o Insomnia ou outra ferramenta similar para testar a API. Você pode importar as configurações do Insomnia a partir do arquivo insomnia.json neste repositório.

### 🔩 Endpoints da API Rest

Para testar a API use Insomnia ou Postman para fazer as requisições.

### Listar usuários da conta:

#### `GET` `/contas?senha_banco=Cubos123Bank`

Retorno (Aqui resumido para fins de demonstração):

```json
[
	{
		"numero": "1",
		"saldo": 220333,
		"usuario": {
			"nome": "Maria Jose",
			"cpf": "00011122233",
			"data_nascimento": "2021-03-15",
			"telefone": "71999998888",
			"email": "foo@bar.com",
			"senha": "1234"
		}
	}	
]
```

### Criar conta:

#### `POST` `/contas`

Requisição via json body.

```json
{
	"nome": "José Maria",
	"cpf": "11111111112",	
	"data_nascimento": "1990-01-01",
	"telefone": "83111111112",
	"email": "josemaria@cubos.com",
	"senha": "1234"
}

```
Retorno:

```
204 No Content
```

### Atualizar usuário da conta:

#### `PUT` `/contas/:numeroConta/usuario`

Requisição via json body.

```json
{
	"nome": "Sicrano Santos Silva",
	"cpf": "11111111112",	
	"data_nascimento": "1990-01-01",
	"telefone": "83111111112",
	"email": "sicrano@santossilva.com",
	"senha": "1234"
}

```

Obs.: No parâmetro da URL :numeroConta coloque o numero da conta que deseja alterar o usuário.

Retorno:

```
204 No Content
```

### Excluir Conta:

#### `DELETE` `/contas/:numeroConta`

Requisição - Sem json body

Na URL:
```
localhost:3000/contas/2?senha_banco=Cubos123Bank
```
Obs.: No parâmetro da URL :numeroConta coloque o numero da conta que deseja excluir.

Retorno:

```
204 No Content
```

### Depositar na conta:

#### `POST` `/transacoes/depositar`

Requisição via json body.

```json
{
	"numero_conta": "2",
	"valor": 250000
}
```

Retorno:

```
204 No Content
```

### Sacar da conta:

#### `POST` `/transacoes/sacar`

Requisição via json body.

```json
{
	"numero_conta": "2",
	"valor": 100000,
	"senha": "1234"
}
```

Retorno:

```
204 No Content
```

### Tranferir saldo entre contas:

#### `POST` `/transacoes/transferir`

Requisição via json body.

```json
{
	"numero_conta_origem": "2",
	"numero_conta_destino": "1",
	"valor": 15000,
	"senha": "1234"
}
```

Retorno:

```
204 No Content
```

### Saldo da conta:

#### `GET` `/contas/saldo?numero_conta=123&senha=123`

Requisição - Sem json body

Na URL:
```
localhost:3000/contas/saldo?numero_conta=2&senha=1234&senha_banco=Cubos123Bank
```

Retorno:

```json
{
	"saldo": 135000
}
```

### Extrato da conta:

#### `GET` `/contas/extrato?numero_conta=123&senha=123`

Requisição - Sem json body

Na URL:
```
localhost:3000/contas/extrato?numero_conta=2&senha=1234&senha_banco=Cubos123Bank
```

Retorno:

```json
{
	"depositos": [
		{
			"data": "2023-09-10 12:19:43",
			"numero_conta": "2",
			"valor": 250000
		}
	],
	"saques": [
		{
			"data": "2023-09-10 12:19:45",
			"numero_conta": "2",
			"valor": 100000
		}
	],
	"transferenciasEnviadas": [
		{
			"data": "2023-09-10 12:19:48",
			"numero_conta_origem": "2",
			"numero_conta_destino": "1",
			"valor": 15000
		}
	],
	"transferenciasRecebidas": [
		{
			"data": "2023-09-10 12:20:15",
			"numero_conta_origem": "1",
			"numero_conta_destino": "2",
			"valor": 10000
		}
	]
}
```

## Tecnologias Utilizadas

* [Node.js](https://nodejs.org/en) - Javascript runtime environment.
* [Express](https://expressjs.com/pt-br/) - Framework para aplicação Web do Node.js.
* [JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript) - Linguagem de programação utilizada neste projeto.

## Autor
**Romão Bernardo** - *Segundo projeto do curso de formação de Backend M02* - [romaobernnardo](https://github.com/romaobernnardo)

## Contribuir para o Projeto

1. Faça um fork do projeto.

2. Crie uma nova branch com as suas alterações:

```
git checkout -b minha-feature
```

3. Salve as alterações e crie uma mensagem de commit descrevendo o que você fez:

```
git commit -m "feat: Minha nova funcionalidade"
```

4. Envie as suas alterações para o GitHub:

```
git push origin minha-feature
```

5. Abra um Pull Request descrevendo suas alterações.

## Licença
Não se aplica a este projeto.

