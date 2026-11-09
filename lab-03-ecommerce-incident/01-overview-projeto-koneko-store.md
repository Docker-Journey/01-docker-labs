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
