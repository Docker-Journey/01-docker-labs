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

---
![Infraestrutura compartilhada](https://img.shields.io/badge/Infraestrutura_compartilhada-F1C40F?style=for-the-badge&labelColor=B7950B)
- Imagem: `koneko-store:v1`
- Volume: `konekostore-data`
- Bind mount: `./logs`
- Banco de dados: `SQLite`
- Linguagem: `Python`

---

## 2. Como a operação vai funcionar

Imagine que uma cliente compre dois mouses Logitech.

<details>
<summary>1. Cliente faz o pedido</summary>

Solicita 2 mouses pela API.

</details>

<details>
<summary>2. API recebe a compra</summary>

Cria o pedido `PED-0001` com status `PENDING`.

</details>

<details>
<summary>3. Worker identifica o pedido</summary>

Busca o pedido pendente no banco.

</details>

<details>
<summary>4. Worker processa a compra</summary>

Confere o estoque e reduz de 30 para 28.

</details>

<details>
<summary>5. Banco registra a operação</summary>

Atualiza o pedido para `PROCESSED`.

</details>

<details>
<summary>6. Cliente recebe a confirmação</summary>

A API consulta o status atualizado.

</details>

> E tudo isso vai deixar rastros: logs, registros no banco, estados dos containers e evidências no Linux.

---

## 3. Nosso plano de construção

<table>
  <thead>
    <tr>
      <th>Etapa</th>
      <th>O que vamos fazer</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center"><strong>01</strong></td>
      <td>Preparar a estrutura do projeto</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>02</strong></td>
      <td>Criar o banco SQLite e cadastrar produtos</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>03</strong></td>
      <td>Construir a API com FastAPI</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>04</strong></td>
      <td>Construir o Worker</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>05</strong></td>
      <td>Criar a imagem Docker</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>06</strong></td>
      <td>Executar API e Worker em containers</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>07</strong></td>
      <td>Configurar volume, bind mount e rede</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>08</strong></td>
      <td>Gerar pedidos e validar o estoque</td>
      <td>🔜</td>
    </tr>
    <tr>
      <td align="center"><strong>09</strong></td>
      <td>Investigar logs e comportamento dos serviços</td>
      <td>🔜</td>
    </tr>

   <tr>
      <td align="center"><strong>10</strong></td>
      <td>Simular incidentes e documentar evidências</td>
      <td>🔜</td>
    </tr>
  </tbody>
</table>

**Uma regra importante**:
primeiro vamos fazer funcionar. Depois vamos provocar falhas controladas. 
Não vamos quebrar algo que ainda não sabemos como funciona.

**Também vamos implementar uma proteção chamada idempotência:** se o Worker tentar
processar o mesmo pedido novamente, o estoque não poderá ser descontado duas vezes.

**Metodologia**

**E vamos seguir no nosso método:**

![Metodologia](https://img.shields.io/badge/Aprender_→_Construir_→_Quebrar_→_Investigar_→_Coletar_evidências_→_Corrigir_→_Validar-228B22?style=for-the-badge&labelColor=145A32)


---

<div align="center">

Se gostou, não esqueça de deixar uma ⭐

<img width="200" alt="Image" src="https://github.com/user-attachments/assets/aca57b06-3ea1-49e4-96fb-b2a00b8f8918" />

</div>
