# Payment API com Flask

Este projeto é uma API de pagamentos desenvolvida em Flask que simula a
criação de um pagamento via Pix. A aplicação gera um QR Code para o
pagamento, exibe uma página de pagamento e uma página de confirmação.

## 🚀 Tecnologias Utilizadas

-   **Python**
-   **Flask**: Micro-framework web para a criação da API e renderização
    das páginas.
-   **Flask-SQLAlchemy**: Para interação com o banco de dados SQLite.
-   **Flask-SocketIO**: Para a criação e utilização do websocket com o Flask
-   **qrcode**: Biblioteca para a geração dos QR Codes de pagamento.
-   **HTML/CSS**: Para a criação das páginas de frontend.

------------------------------------------------------------------------

## 🖥️ Telas do Frontend

### **Página de Pagamento (Pedido Realizado)**

<img src="https://ik.imagekit.io/brunogodoy/Aguardando" alt="Tela Aguardando pagamento" width="800"/>

### **Página de Confirmação (Pedido Confirmado)**

<img src="https://ik.imagekit.io/brunogodoy/Aprovado" alt="Tela Pagamento Aprovado" width="800"/>

### **Página 404 (Não Encontrado)**

<img src="https://ik.imagekit.io/brunogodoy/404" alt="Tela 404" width="800"/>

------------------------------------------------------------------------

## 🛠️ Descrição dos Endpoints

### 1. **Criar Pagamento Pix**

**Método:** POST\
**URL:** `/payments/pix`

Cria um novo registro de pagamento no banco de dados.

#### Corpo da Requisição (JSON)

``` json
{
  "value": 100.50
}
```

#### Ação

-   Recebe o valor do pagamento.
-   Define expiração de 30 minutos.
-   Usa a classe `Pix` para gerar `bank_payment_id` e hash.
-   Gera um QR Code em PNG e salva em `static/img/`.
-   Salva tudo no banco.

#### Resposta

``` json
{
  "message": "The payment has been created",
  "payment": {
    "id": 1,
    "value": 100.50,
    "paid": false,
    "bank_payment_id": "uuid-...",
    "qr_code": "qr_code_payment_uuid-...",
    "expiration_date": "..."
  }
}
```

------------------------------------------------------------------------

### 2. **Exibir Página de Pagamento**

**Método:** GET\
**URL:** `/payments/pix/<int:payment_id>`

Renderiza a página HTML do pagamento, mostrando QR Code e valor.

------------------------------------------------------------------------

### 3. **Servir Imagem do QR Code**

**Método:** GET\
**URL:** `/payments/pix/qr_code/<file_name>`

Retorna a imagem PNG correspondente ao QR Code.

------------------------------------------------------------------------

### 4. **Confirmação de Pagamento (Webhook)**

**Método:** POST\
**URL:** `/payments/pix/confirmation`

Simula um webhook de confirmação.

#### Resposta

``` json
{
  "message": "The payment has been confirmed"
}
```

------------------------------------------------------------------------

## ⚙️ Como Executar o Projeto

### 1. Clone o repositório

``` bash
git clone https://github.com/Brunogodoy2911/Payment_API
cd Payment_API
```

### 2. Crie e ative o ambiente virtual

``` bash
# Windows
python -m venv venv
.env\Scriptsctivate

# Linux/Mac
python3 -m venv venv
source venv/bin/activate
```

### 3. Instale as dependências

``` bash
pip install -r requirements.txt
```

### 4. Execute a aplicação

``` bash
python app.py
```

A aplicação estará rodando em **http://127.0.0.1:5000**.

### 5. Criar o banco de dados

Se necessário:

``` python
from repository.database import db
from app import app
with app.app_context():
    db.create_all()
```
