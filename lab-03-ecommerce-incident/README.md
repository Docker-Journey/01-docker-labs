<div align="center">
<img src="https://github.com/user-attachments/assets/760641e2-0a20-405a-8215-9ef381d3f618" width="320" alt="logo-techstore"/>

# Koneko Store 
Loja virtual de produtos de tecnologia

</div>

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![DevOps](https://img.shields.io/badge/DevOps-5C2D91?style=for-the-badge&logo=azuredevops&logoColor=white)
![Troubleshooting](https://img.shields.io/badge/Troubleshooting-FFB703?style=for-the-badge&logo=ifixit&logoColor=black)
![Observability](https://img.shields.io/badge/Observability-8A2BE2?style=for-the-badge&logo=grafana&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)

## Sobre o projeto

Este projeto é um laboratório de infraestrutura que simula a operação de uma pequena loja virtual, desde o recebimento do pedido até a atualização do estoque.
Vamos construir um laboratório de Docker + Linux Troubleshooting.

**Nossa metodologia será:**

![Metodologia](https://img.shields.io/badge/Aprender_→_Construir_→_Quebrar_→_Investigar_→_Coletar_evidências_→_Corrigir_→_Validar-6C3483?style=for-the-badge&labelColor=45205D)

E vamos provocar falhas de propósito. Inclusive apagar containers, interromper processos, perder arquivos de configuração e provocar problemas de persistência.

## Sobre a Koneko Store

A Koneko Store é uma loja virtual de technologia

**Ela vende:**

- Notebooks.
- Teclados.
- Mouses.
- Monitores.
- Headsets.

Um cliente acessa a loja, faz um pedido, o sistema registra a compra, atualiza o estoque e gera um registro da operação.

Agora imagine que essa operação começa a apresentar problemas.

- Clientes não conseguem finalizar compras.
- Pedidos desaparecem.
- O estoque apresenta valores incorretos.
- O serviço de processamento de pedidos para.
- Os arquivos de log deixam de ser atualizados.
- Um container é excluído.
- O sistema volta a funcionar, mas os dados não aparecem.
É esse ambiente que vamos investigar.


## Arquitetura

Vamos construir uma imagem Docker própria, com uma aplicação de e-commerce desenvolvida em Python.
A imagem será nossa receita para criar containers.

--- 

<div align="center">
<img src="https://github.com/user-attachments/assets/88b0a673-8ec4-4580-afc3-a6df283cd612"  width="800" alt="AWS Logo"/>
</div>


