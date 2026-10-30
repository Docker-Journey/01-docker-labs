<div align="center">
<img src="https://github.com/user-attachments/assets/2f6e9ba5-2baf-475f-8e33-357c4f67245a" width="220" alt="AWS Logo"/>

# Supermarket Stock API 

**DevOps Lab**

</div>

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

Projeto prático desenvolvido para consolidar conceitos fundamentais e intermediários de **Docker**, **sistemas de arquivos**, **persistência de dados (Volumes e Bind Mounts)**, **documentação OpenAPI/Swagger** e **troubleshooting de ecossistemas conteinerizados** no cenário de varejo/supermercados.

## 🛒 Objetivo do projeto

Criaremos uma API simples em Python (Flask) para cadastro de produtos de supermercado. 
Ela salvará:
- logs de acessos em um **Bind Mount**
- e o inventário de produtos em um **Docker Volume**.

Ao destruir o primeiro container (`stock_app`) e subir uma nova instância (`stock_app_v2`) reconectada ao mesmo volume
e  bind mount, mostraremos a regra central de infraestrutura conteinerizada:

<div align="center">
<img src="https://github.com/user-attachments/assets/965d3c2e-a56a-4718-a5cb-7656cdc75006"  width="800" alt="AWS Logo"/>
</div>

---

## O que será criado

- Criaremos a imagem `supermarket-api:v1`
- Criaremos o container 1 `stock_app`
- Criaremos um volume `supermarket-stock_data` 
- Criaremos o Bind Mount: espelhando o diretorio do host (`~/supermarket-lab/log`) para o diretorio no container 1 (`/app/logs`)

## Situação

- Destruiremos o container 1 `stock_app`
- Criaremos o container 2 `stock_app_v2`
- Anexaremos o volume `supermarket-stock_data` ao novo container 2 `stock_app_v2`

## 🛒 Infográfico

No infográfico, a gente  consegue visualizar exatamente a divisão que criaremos: 
como as portas conectam o navegador à API e a diferença do Bind Mount (que joga o log direto para o diretorio no Ubuntu)
e do Docker Volume (que segura o banco de dados/estoque de forma persistente e segura).

<div align="center">
<img src="https://github.com/user-attachments/assets/1ad34c39-e5b7-49f3-b217-0cc8bb6421c4" width="800" alt="AWS Logo"/>
</div>

---

## 🛒 Como a arquitetura do projeto foi estruturada?

**1. Imagem e Containerização (`supermarket-api:v1`):**

- Criada a partir de um Dockerfile otimizado.

- Executada com um usuário não-root por boas práticas de segurança (`novo_user`).

- Roda em um container isolado (`stock_app` / `stock_app_v2`).

**2. Serviço RESTful + Swagger UI:**

- Expõe as rotas `/health` (status da API) e `/products` (listagem e cadastro de produtos em JSON).

- Porta do container (5000) mapeada para a porta do host Ubuntu (8080), permitindo acessar a documentação interativa pelo navegador em `http://localhost:8080/apidocs/`.

**3. Estratégia de Persistência de Dados**

**01. Docker Volume (`supermarket_stock_data`):**

- Mapeamento: `/app/data/` interno do container.

- Arquivo: `inventory.json`.

- Finalidade: Armazenar os dados de produtos e estoque em um local isolado, de alta performance e totalmente protegido contra falhas ou recriações do container.

**02. Bind Mount ($(pwd)/logs):**

- Mapeamento: `~/supermarket-lab/logs/ (Ubuntu Host) ➔ /app/logs/ (Container)`.

- Arquivo: `access.log`.

- Finalidade: Gravar em tempo real os logs de acesso e auditoria da API diretamente no sistema de arquivos da máquina física (Ubuntu),
permitindo consulta imediata sem precisar acessar o container.

**Sintaxe do bind mount: anexando o volume ao container**

<div align="center">
<img src="https://github.com/user-attachments/assets/ca06b364-b8ed-4ae4-a816-47087b5e82f2" width="500" alt="BindMount"/>
</div>

---

## Ciclo de vida e persistência

Esta imagem abaixo visualiza como a Imagem Docker (`supermarket-api:v1`) gera os nossos dois containers, mostrando que,
mesmo com a destruição do `stock_app` via terminal, o banco de dados (inventory.json e os logs de acessos) 
continuou intacto e conectado ao `stock_app_v2` devido ao volume e ao bind mount.

<div align="center">
<img src="https://github.com/user-attachments/assets/6e3a1c94-b318-43b1-9743-ee21fad8384a" width="500" alt="AWS Logo"/>
</div>

---

## 🛒 Objetivos técnicos do Projeto

- [x] **API RESTful com Documentação Swagger (OpenAPI):** Endpoints modernos com UI interativa em `/apidocs/`.
- [x] **Construção de Imagens Seguras:** `Dockerfile` executando com usuário não-root (`totvs_user`) e otimização de cache.
- [x] **Mapeamento de Portas:** Exposição do serviço web (`-p 8080:5000`).
- [x] **Persistência de Dados Híbrida:**
  - **Docker Volume (`supermarket_stock_data`):** Armazenamento isolado de alta performance para o inventário de produtos.
  - **Bind Mount (`./logs`):** Espelhamento do host (Ubuntu) para auditoria e verificação de logs de acesso em tempo real.
- [x] **Observabilidade e Diagnóstico DevOps:** Inspeção profunda usando `docker inspect`, `docker logs`, `docker diff` e `docker exec`.

---

## 🛠️ Tecnologias Utilizadas

- **Sistema Operacional Base:** Ubuntu Linux
- **Linguagem / Framework:** Python 3.10 / Flask + Flasgger (Swagger UI) / API RESTful
- **Container Engine:** Docker Engine (CLI)
- **Ferramentas de Teste:** Swagger UI, `curl`, `jq`

---

## 📂 Estrutura do Repositório

```text
lab-supermarket-lab/
├── app/
│   ├── app.py              # API RESTful com rotas e anotações Swagger
│   └── requirements.txt    # Dependências (Flask + Flasgger)
├── logs/                   # Diretório do Host para Bind Mount (Logs)
├── Dockerfile              # Build da imagem Docker otimizada
└── README.md               # Documentação do laboratório
```

