# 01 - Preparar estrutura do projeto

Nosso objetivo

```
koneko-store/
├── app/
├── database/
├── logs/
└── worker/
```

## Missão 01 - Preparar o terreno

Primeiro, vamos apenas verificar o ambiente. Não vamos modificar nenhum projeto existente.

Execute um comando por vez:

```
whoami
```
> Ele mostra qual usuário está executando os comandos.

Depois:

```
pwd
```

> Mostra onde estamos no sistema de arquivos.

E, por último:
```
docker --version
```

> Confirma a versão do Docker instalada.

## Missão 02 - Criar o diretório do projeto

Vamos criar uma pasta exclusiva para a Koneko Store.

Execute:
```
mkdir -p ~/koneko-store
```

**que esse comando faz?**

- mkdir: cria diretórios.
- -p: evita erro caso o diretório já exista e permite criar estruturas de diretórios.
- ~: representa /home/vboxuser.

Agora entre na pasta:
```
cd ~/koneko-store
```

E confira:
```
pwd
```
Por último, confira se ela está vazia:
```
ls -lah
```

> Queremos começar com um diretório limpo.

**Nesse lab nós vamos aprender a distinguir três coisas:**

1. O projeto e seus arquivos no Linux.
2. A imagem Docker que empacota a aplicação.
3. Os containers que executam os serviços.
Essa separação será muito importante quando começarmos a simular incidentes.
