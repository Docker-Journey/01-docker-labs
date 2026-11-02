 <div align="center">

# 🛒 API de Gestão de Estoque

### Segurança dos dados, continuidade da operação e auditoria em tempo real

**Uma solução de gestão de estoque para supermercados utilizando Python e Docker.**

![Docker](https://img.shields.io/badge/Docker-Container-2496ED?logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-API-3776AB?logo=python&logoColor=white)
![Status](https://img.shields.io/badge/Projeto-Em%20desenvolvimento-8957E5)

</div>

---

## Sobre o projeto

Desenvolvemos uma solução para gestão de estoque de supermercados utilizando **Python e Docker**.

Para facilitar o entendimento, imagine o Docker como uma **caixa organizada e isolada (Container)**. Dentro dela, nossa aplicação funciona com todos os recursos necessários para cadastrar e consultar produtos.

Através de uma interface de testes da API, conseguimos realizar duas operações principais:

- **POST:** cadastrar novos produtos no estoque, como arroz, feijão e café.
- **GET:** consultar os produtos cadastrados e visualizar as informações do estoque.

<div align="center">

<img src="https://github.com/user-attachments/assets/6dcf034b-3784-4be4-b8eb-a1ef8133c110" width="85%" alt="Ilustração da solução de gestão de estoque utilizando Docker"/>

<sub>Figura 1 — Aplicação de gestão de estoque executando em um Container Docker.</sub>

</div>

---

## Segurança e organização dos dados

Para garantir a segurança das informações do supermercado, organizamos os dados em duas estruturas estratégicas:

### 1. Dados dos produtos - Docker Volume

**Onde são armazenados?**

Em um Docker Volume, que funciona como um cofre de dados independente do Container.

**Por que isso é importante?**

Imagine que a aplicação apresente uma falha, o computador seja desligado ou o Container precise ser reiniciado.

Os dados do estoque continuam preservados no Volume. Quando a aplicação volta a funcionar, ela se conecta novamente ao armazenamento e recupera as informações.

**Benefício para o negócio:** continuidade da operação e proteção dos dados do estoque.

### 2. Registros de acesso - Bind Mount

**Onde são armazenados?**

Em uma pasta do próprio computador, compartilhada com o Container por meio de um Bind Mount.

**Por que isso é importante?**

Cada vez que alguém cadastra um produto ou consulta o estoque, o sistema registra informações como:

- Data e horário da operação.
- Tipo de ação realizada.
- Registro da atividade executada.

Esses registros ficam disponíveis diretamente no computador, permitindo que a equipe consulte o histórico sem precisar acessar ou interromper a aplicação principal.

**Benefício para o negócio:** transparência, rastreabilidade e facilidade para auditorias.

<div align="center">

<img src="https://github.com/user-attachments/assets/7262ce77-4b00-4f96-a640-57a0d044bd24" width="85%" alt="Ilustração da proteção dos dados com Docker Volume e Bind Mount"/>

<sub>Figura 2 — Estratégia de armazenamento: Docker Volume e Bind Mount.</sub>

</div>

---

## Resumo: onde cada informação fica?

| Estrutura | O que armazena? | Benefício |
|:---|:---|:---|
| **Docker Volume** | Dados dos produtos e estoque | Preservação das informações mesmo após reinicializações do Container. |
| **Bind Mount** | Logs e registros de acesso | Consulta do histórico diretamente no computador. |

<div align="center">

**🛡️ Dados protegidos + 📋 Histórico de operações + 🛒 Continuidade do negócio**

</div>

---

## Como testar a aplicação?

A seguir, o roteiro para executar e validar a solução.

### 1. Construir a imagem Docker

Na pasta do projeto, execute:

```bash
docker build -t supermarket-api:v1 .
```

### 2. Criar o Volume

```bash
docker volume create stock_data
```

### 3. Criar e executar o Container

```bash
docker run -d \
  --name stock_app \
  -p 8080:5000 \
  -v stock_data:/app/data \
  -v ~/supermarket-lab/logs:/app/logs \
  supermarket-api:v1
```

### 4. Verificar se os recursos foram criados

**Listar imagens Docker:**

```bash
docker images
```

**Verificar Containers em execução:**

```bash
docker ps
```

### 5. Acessar a interface de testes da API

Abra o navegador e acesse:

```text
http://localhost:8080/apidocs/
```

Na interface, vamos testar o cadastro e a consulta dos produtos.
<div align="center">

<img src="https://github.com/user-attachments/assets/e2bad714-b950-42a3-9c69-52c4dd0ba1c1" width="85%" alt="Interface da API"/>

<sub>Figura 3 - Interface da API.</sub>

</div>

### 6. Cadastrar um produto - POST

Selecione o método **POST**, clique em `Try it out` e insira o seguinte conteúdo no campo `Request body`:

```json
{
  "code": "789123",
  "name": "Arroz tipo 2 kg",
  "price": 28.90,
  "stock": 150
}
```






Clique em **Execute** para enviar a solicitação.

Resultado esperado: produto cadastrado com sucesso!

### 7. Consultar os produtos — GET

Agora, selecione o método **GET** na interface da API.

1. Clique em `Try it out`.
2. Clique em `Execute`.
3. Verifique a resposta apresentada pela API.

Resultado esperado: a aplicação retorna a lista de produtos cadastrados no estoque.

<div align="center">

**📸 Resultado dos testes**

*Adicione aqui a captura de tela da interface da API mostrando as operações POST e GET.*

</div>

---

## 💡 Resultado esperado

Com essa solução, demonstramos como a tecnologia pode contribuir diretamente para a operação de um supermercado:

- **Segurança:** os dados do estoque permanecem armazenados mesmo quando o Container é reiniciado.
- **Auditoria:** os registros de acesso ficam disponíveis para consulta.
- **Organização:** cada tipo de informação possui seu próprio local de armazenamento.
- **Continuidade:** a aplicação pode ser reiniciada sem perder os dados persistidos.

<div align="center">

### Tecnologia trabalhando a favor do negócio! 💜

*Uma solução simples para demonstrar como Docker, Python e estratégias de armazenamento podem apoiar a gestão e a segurança de uma operação.*

</div>
