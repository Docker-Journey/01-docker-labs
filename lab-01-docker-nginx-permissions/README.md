# Lab 01: Deploy de Aplicação Web em Docker, Permissões Linux e Troubleshooting de Logs

## Objetivo
Este laboratório prático tem como objetivo simular um cenário real de infraestrutura e DevOps:
a implantação de uma aplicação web estática utilizando **Docker** e **Nginx** sobre um sistema
operacional **Linux**, abordando o diagnóstico e resolução de erros de permissão de acesso (`403 Forbidden`)
através da análise de logs de container.

---

## Tecnologias e Ferramentas Utilizadas
* **Sistema Operacional:** Linux (Ubuntu/Debian rodando em VM no Oracle VirtualBox)
* **Containerização:** Docker (Instalação via `apt`)
* **Servidor Web:** Nginx (Imagem Oficial do Docker Hub)
* **Ferramentas de CLI:** Terminal Bash, `curl`, `chmod`, `docker exec`, `docker logs`

---

## Passo a Passo da Execução

### 1. Preparação do Ambiente e Sistema de Arquivos
Navegação até o diretório pessoal do usuário e criação da estrutura de pastas para a aplicação:

```bash
# Navegar para a Home do usuário
cd ~

# Criar a estrutura de diretórios do projeto
mkdir -p projetos/meu_app
cd projetos/meu_app

# Criar a página HTML inicial da aplicação
echo "<h1>Minha aplicacao rodando no Docker!</h1>" > index.html
```
### 2. Gestão de Permissões no Linux (chmod)
Para simular uma falha de acesso comum em ambientes de produção,
a permissão do arquivo `index.html` foi restrita para
leitura/escrita apenas pelo proprietário (600):
```bash
# Alterar permissão para apenas o dono (Read/Write)
chmod 600 index.html

# Verificar atributos e permissões do arquivo (-rw-------)
ls -la index.html
```
### 3. Instalação e Execução do Container Docker
Instalação do engine do Docker via gerenciador
de pacotes apt e inicialização do servidor Nginx mapeando porta e volume:

```Bash
# Instalação automatizada do Docker
sudo apt update
sudo apt install -y docker.io

# Subir o container em segundo plano mapeando porta 8080:80 e montando o volume
docker run -d --name meu_webserver -p 8080:80 -v $(pwd)/index.html:/usr/share/nginx/html/index.html:ro nginx

# Verificar status do container ativo
docker ps
```

<img width="800" height="500" alt="Image" src="https://github.com/user-attachments/assets/b89677df-7f56-41de-8727-883037482914" />


### 4. Troubleshooting: Diagnóstico e Resolução do Erro 403 Forbidden

A. Identificação da Falha

Ao testar a aplicação com o comando curl, o servidor Nginx respondeu com o código HTTP 403 Forbidden:

```
curl http://localhost:8080
```
> Sintoma: O container estava rodando, mas o Nginx não conseguia ler o arquivo index.html montado no volume.

B. Análise de Logs do Container

Inspeção dos registros de evento do container para identificar a causa raiz:
```
docker logs meu_webserver
```
Log identificado:

```
[error] 29#29: *1 open() "/usr/share/nginx/html/index.html" failed (13: Permission denied)
```

C. Inspeção Interna do Container (docker exec)

Acesso interativo ao shell do container para validar permissões do sistema de arquivos interno:

```bash
# Entrar no container em modo interativo
docker exec -it meu_webserver sh

# Verificar permissões no diretório do Nginx
cd /usr/share/nginx/html
ls -la
exit
```

D. Correção e Validação

Ajuste da permissão do arquivo no host para 644 
(permitindo leitura por outros usuários e pelo processo do Nginx):

```bash
# Conceder permissão de leitura para outros usuários
chmod 644 index.html

# Testar acesso à aplicação novamente
curl http://localhost:8080
```
Resultado: 
```
Resposta de sucesso 200 OK exibindo o conteúdo <h1>Minha aplicacao rodando no Docker!</h1>.
```

<img width="800" height="600" alt="Image" src="https://github.com/user-attachments/assets/a69143d9-2950-48f8-9b35-200a995e26e2" />

### Principais Aprendizados (Key Takeaways)
1. Mapeamento de Volumes e Permissões: O processo interno de um container
herda as restrições de permissão do sistema de arquivos do host.

2. Ciclo de Troubleshooting:
Diagnóstico guiado por requisição (`curl`) -> inspeção de logs de erro (`docker logs`) -> navegação interna no container (`docker exec`) -> correção de causa raiz (`chmod`).

3. Automação de CLI: Entendimento do papel das flags -y no apt (automação CI/CD) e
do script docker-entrypoint.sh na inicialização do container.
