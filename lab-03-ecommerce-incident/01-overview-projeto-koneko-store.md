<div align="center">

<img src="https://github.com/user-attachments/assets/eaf526f7-8d56-4a3f-9829-ffd58207d50f" width="50%" alt="Koneko Store - Loja virtual de produtos de tecnologia"/>

# Koneko Store

**Loja virtual de produtos de tecnologia**

O que vamos construir

</div>

## 0. Relembrando

primeiro vamos fazer funcionar.
Depois vamos provocar falhas controladas. 
Não vamos quebrar algo que ainda não sabemos como funciona.

**Idempotência**

Também vamos implementar uma proteção chamada idempotência: se o Worker tentar processar o mesmo pedido novamente,
o estoque não poderá ser descontado duas vezes.

## 1. Nossa loja terá dois serviços principais:

![Serviço-1](https://img.shields.io/badge/Serviço--1-28A745?style=for-the-badge&labelColor=19692C) koneko-api



A porta de entrada da loja. Recebe as solicitações dos clientes.
- Consultar produtos.
- Consultar estoque.
- Receber pedidos.
- Registrar pedidos como `PENDING`.
- Consultar o status de um pedido.

---

![Serviço-2](https://img.shields.io/badge/Serviço--2-2980B9?style=for-the-badge&labelColor=1F618D) koneko-worker

O trabalhador dos bastidores. Processa os pedidos.
- Busca pedidos pendentes.
- Verifica disponibilidade.
- Atualiza o estoque.
- Marca pedidos como `PROCESSED`.
- Registra logs do processamento.

