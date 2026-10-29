# 🛒 Supermarket Stock API 
DevOps Lab (REST API + Swagger UI)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

Projeto prático desenvolvido para consolidar conceitos fundamentais e intermediários de **Docker**, **sistemas de arquivos**, **persistência de dados (Volumes e Bind Mounts)**, **documentação OpenAPI/Swagger** e **troubleshooting de ecossistemas conteinerizados** no cenário de varejo/supermercados.

---

## 📌 Objetivos do Projeto

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

