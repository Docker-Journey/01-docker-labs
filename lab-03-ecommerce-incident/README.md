<div align="center">

<img src="https://github.com/user-attachments/assets/eaf526f7-8d56-4a3f-9829-ffd58207d50f" width="100%" alt="Koneko Store - Loja virtual de produtos de tecnologia"/>

# Koneko Store

**Loja virtual de produtos de tecnologia**

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
<img src="https://github.com/user-attachments/assets/88b0a673-8ec4-4580-afc3-a6df283cd612"  width="800" alt="Diagrama"/>
</div>

---

## 1. Jornada do pedido

A jornada do pedido (Order Lifecycle) é o caminho que um pedido percorre dentro de um e-commerce,
desde o momento em que o cliente escolhe um produto até a conclusão da compra.

E essa jornada será o coração do nosso laboratório Koneko Store.

**1. A jornada do cliente dentro da Koneko Store**

<div align="center">
<img src="https://github.com/user-attachments/assets/87de2a41-f60f-49fa-925e-f32bc50c42a7"  width="800" alt="jornada do pedido"/>
</div>


Imagine esta sequência:

1. Cliente acessa a loja.
2. Consulta os produtos disponíveis.
3. Escolhe um produto.
4. Adiciona ao carrinho.
5. Finaliza a compra.
6. O sistema registra o pedido.
7. O pedido é processado.
8. O estoque é atualizado.
9. O pedido é confirmado.
10. O produto é separado e enviado.
    
Mas existe uma diferença importante: o cliente faz a compra, a `API` registra o pedido e o `Worker` processa esse pedido.


## 2. Jornada dentro da arquitetura Docker

Imagine que a cliente compre dois mouses Logitech.

**Fluxo de processamento do pedido**

1️⃣ Cliente escolhe o produto

<table>
  <tr>
    <td width="50%" align="center" valign="middle">
      <img src="https://github.com/user-attachments/assets/47c728ff-2f37-466c-91b2-1d37e7e6b8c7"  alt="Mouse Gamer" width="60%">
    </td>
    <td width="50%" valign="middle">
      <h3>Mouse Logitech</h3>
      <p><strong>Quantidade:</strong> 2 unidades</p>
      <p><strong>Preço unitário:</strong> R$ 120,00</p>     
    </td>
  </tr>
</table>

>  O cliente adiciona dois mouses ao carrinho e finaliza a compra.

2️⃣ API recebe o pedido

`koneko-api`

<div align="center">
<img src="https://github.com/user-attachments/assets/8ff88e98-491d-4da9-9ef0-3645cd66133d" width="600" alt="Diagrama"/>
</div>

> A API recebe os dados da compra, valida se o produto existe e verifica a disponibilidade inicial do estoque.
> Exemplo: o estoque informa 30 unidades.

3️⃣ Pedido é registrado

`koneko-api`

A API gera um identificador:

`PED-0001`

<div align="center">
<img src="https://github.com/user-attachments/assets/d00357f1-ea52-47f7-8b2e-afc7a3a5a396" width="600" alt="Diagrama"/>
</div>

> O pedido é armazenado com status `PENDING`.

4️⃣ Worker recebe o pedido para processamento

`koneko-worker`

<div align="center">
<img src="https://github.com/user-attachments/assets/86b2ac6a-213b-421d-bb3c-4cc293b49455" width="600" alt="Diagrama"/>
</div>



> O Worker busca os pedidos pendentes, verifica as regras de processamento e executa a atualização do estoque.

5️⃣ Estoque é atualizado

`koneko-worker`

- Antes: 30 mouses.

- Saída: 2 mouses.

- Depois: 28 mouses.

<div align="center">
<img src="https://github.com/user-attachments/assets/d9d3588b-09f6-4c87-ad97-d90cb8b12b95" width="600" alt="Diagrama"/>
</div>


6️⃣ Pedido é atualizado
   
Processamento concluído

O status muda de `PENDING` para `PROCESSED`.


<div align="center">
<img src="https://github.com/user-attachments/assets/1f1b4bf4-395a-41c5-af0d-6530f450380e" width="600" alt="Diagrama"/>
</div>

7️⃣ Cliente recebe a confirmação

> A loja informa que o pedido foi confirmado e segue para a próxima etapa logística:
> separação e envio.

<div align="center">
<img src="https://github.com/user-attachments/assets/ce635371-d1de-4fe5-89aa-6b29b452a30f" width="600" alt="Diagrama"/>
</div>

## 3. O que vai acontecer com os dados em cada etapa?

| **Momento**           | **Serviço** | **O que acontece**       |
|-----------------------|-------------|--------------------------|
| Consulta de produtos  | API         | Lê o catálogo             |
| Finalização da compra | API         | Recebe os dados           |
| Registro do pedido    | API         | Salva o pedido             |
| Processamento         | Worker      | Busca pedidos pendentes   |
| Baixa no estoque      | Worker      | Atualiza quantidade       |
| Confirmação           | Worker/API  | Atualiza status           |
| Auditoria             | Ambos       | Registram logs            |

## 4. Onde ficarão os arquivos?

- Volume Docker: `konekostore-data`

Persistência dos dados da operação

```
konekostore-data/
├── products.db
├── orders.db
├── inventory.db
└── processing.db
```

- Bind mount: `./logs`
Arquivos físicos no Ubuntu / host
```
logs/
├── api.log
├── worker.log
└── errors.log
```





