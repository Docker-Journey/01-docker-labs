## 1. Criar a Estrutura de Diretórios

```Bash
mkdir -p ~/supermarket-lab/app ~/supermarket-lab/logs
cd ~/supermarket-lab
```

## 2. Criar o Arquivo de Dependências (`app/requirements.txt`)
```
cat << 'EOF' > app/requirements.txt
flask==3.0.0
flasgger==0.9.7.1
EOF
```
## 3. Criar o Código da API Python (`app/app.py`)

```Bash
cat << 'EOF' > app/app.py
from flask import Flask, jsonify, request
from flasgger import Swagger
import os
import json
from datetime import datetime

app = Flask(__name__)

swagger_config = {
    "headers": [],
    "specs": [
        {
            "endpoint": 'apispec',
            "route": '/apispec.json',
            "rule_filter": lambda rule: True,
            "model_filter": lambda tag: True,
        }
    ],
    "static_url_path": "/flasgger_static",
    "swagger_ui": True,
    "specs_route": "/apidocs/"
}

template = {
    "swagger": "2.0",
    "info": {
        "title": "Supermarket Stock API",
        "description": "API RESTful para controle de estoque com suporte a Docker Volumes e Bind Mounts.",
        "version": "1.0.0"
    }
}

swagger = Swagger(app, config=swagger_config, template=template)

DATA_FILE = '/app/data/inventory.json'
LOG_FILE = '/app/logs/access.log'

def log_access(action):
    os.makedirs('/app/logs', exist_ok=True)
    with open(LOG_FILE, 'a') as f:
        f.write(f"[{datetime.now().isoformat()}] Ação executada: {action}\n")

def load_inventory():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, 'r') as f:
            return json.load(f)
    return []

def save_inventory(data):
    os.makedirs('/app/data', exist_ok=True)
    with open(DATA_FILE, 'w') as f:
        json.dump(data, f, indent=2)

@app.route('/health', methods=['GET'])
def health():
    """
    Health Check do Serviço
    ---
    responses:
      200:
        description: Retorna o status de saúde da API RESTful.
    """
    log_access("HEALTH_CHECK")
    return jsonify({"status": "OK", "service": "Supermarket Stock API"}), 200

@app.route('/products', methods=['GET'])
def get_products():
    """
    Listar Produtos em Estoque
    ---
    responses:
      200:
        description: Lista completa dos produtos cadastrados.
    """
    log_access("PRODUCTS_LISTED")
    return jsonify(load_inventory()), 200

@app.route('/products', methods=['POST'])
def add_product():
    """
    Cadastrar Novo Produto no Estoque
    ---
    parameters:
      - name: body
        in: body
        required: true
        schema:
          type: object
          properties:
            code:
              type: string
              example: "789123"
            name:
              type: string
              example: "Arroz Tipo 1 5kg"
            price:
              type: number
              example: 28.90
            stock:
              type: integer
              example: 150
    responses:
      201:
        description: Produto cadastrado com sucesso.
    """
    item = request.json
    inventory = load_inventory()
    inventory.append(item)
    save_inventory(inventory)
    log_access(f"PRODUCT_ADDED: {item.get('name')}")
    return jsonify({"message": "Produto cadastrado com sucesso!"}), 201

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
EOF
```

## 4. Criar o Dockerfile com Usuário Sem Root (`novo_user`)

```Bash
cat << 'EOF' > Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app/ .

RUN useradd -m novo_user && \
    mkdir -p /app/data /app/logs && \
    chown -R novo_user:novo_user /app

USER novo_user

EXPOSE 5000

CMD ["python", "app.py"]
EOF
```

## Build e Execução

1. Construir a Imagem
```Bash
docker build -t supermarket-api:v1 .
```
2. Criar o Volume Gerenciado
```Bash
docker volume create supermarket_stock_data
```
3. Subir o Container (Mapeamento de Porta + Volume + Bind Mount)

```Bash
docker run -d \
  --name stock_app \
  -p 8080:5000 \
  -v supermarket_stock_data:/app/data \
  -v $(pwd)/logs:/app/logs \
  supermarket-api:v1
  ```

## Validação do Fluxo
1. Acessar a documentação no navegador: http://localhost:8080/apidocs/

2. Gerar o log e verificar no terminal:

```Bash
curl http://localhost:8080/health
cat ~/supermarket-lab/logs/access.log
```








