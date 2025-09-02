# VACA - Sistema de Análise e Correção Automatizada

Este é um projeto de microsserviços composto por três aplicações principais: API backend (vaca-api), serviço de OMR (vaca-omr) e interface web (vaca-web).

## 📋 Pré-requisitos

- [Docker](https://www.docker.com/get-started) e [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/downloads)

## 🚀 Configuração e Instalação

### 1. Clone este repositório base

```bash
git clone https://github.com/segabrielcarvalho/vaca-app.git
cd vaca-app
```

### 2. Crie a pasta apps e clone os repositórios

Primeiro, crie a pasta `apps` se ela não existir:

```bash
mkdir -p apps
cd apps
```

Clone os repositórios das aplicações dentro da pasta `apps`:

```bash
# Clone do repositório da API
git clone https://github.com/segabrielcarvalho/vaca-api.git

# Clone do repositório do serviço OMR
git clone https://github.com/segabrielcarvalho/vaca-omr.git

# Clone do repositório do frontend web
git clone https://github.com/segabrielcarvalho/vaca-web.git
```

### 3. Configure as variáveis de ambiente

Cada aplicação possui um arquivo `.env.example` com as variáveis de ambiente necessárias. Você precisa copiar o conteúdo desses arquivos para os respectivos arquivos de configuração na pasta `config/`.

#### Para vaca-api:
```bash
# Copie o conteúdo do arquivo apps/vaca-api/.env.example
# e cole no arquivo config/vaca-api.env
cp apps/vaca-api/.env.example ../config/vaca-api.env
```

#### Para vaca-omr:
```bash
# Copie o conteúdo do arquivo apps/vaca-omr/.env.example
# e cole no arquivo config/vaca-orm.env
cp apps/vaca-omr/.env.example ../config/vaca-orm.env
```

#### Para vaca-web:
```bash
# Copie o conteúdo do arquivo apps/vaca-web/.env.example
# e cole no arquivo config/vaca-web.env
cp apps/vaca-web/.env.example ../config/vaca-web.env
```

#### Para o banco de dados:
Edite o arquivo `config/vaca-database.env` com as configurações do PostgreSQL:
```env
POSTGRES_DB=vaca
POSTGRES_USER=vaca
POSTGRES_PASSWORD=vaca123
```

#### Para o LocalStack:
Edite o arquivo `config/vaca-localstack.env` conforme necessário:
```env
SERVICES=s3
DEBUG=1
DATA_DIR=/var/lib/localstack/data
```

### 4. Execute o projeto

Volte para a pasta raiz do projeto e execute:

```bash
cd ..
docker-compose up
```

Para executar em background:
```bash
docker-compose up -d
```

## 📊 Serviços e Portas

Após a execução, os seguintes serviços estarão disponíveis:

| Serviço | Porta | URL | Descrição |
|---------|-------|-----|-----------|
| vaca-api | 11000 | http://localhost:11000 | API Backend |
| vaca-omr | 11001 | http://localhost:11001 | Serviço de OCR/OMR |
| vaca-web | 11002 | http://localhost:11002 | Interface Web |
| PostgreSQL | 5432 | localhost:5432 | Banco de dados |
| Redis | 6379 | localhost:6379 | Cache/Session Store |
| LocalStack | 4566 | http://localhost:4566 | AWS Services Local |

## 🗂️ Estrutura do Projeto

```
vaca/
├── docker-compose.yml          # Configuração dos containers
├── README.md                   # Este arquivo
├── apps/                       # Aplicações (clonadas separadamente)
│   ├── vaca-api/              # API Backend (Node.js/NestJS)
│   ├── vaca-omr/              # Serviço OCR/OMR (Python)
│   └── vaca-web/              # Frontend Web
├── config/                     # Arquivos de configuração
│   ├── vaca-api.env
│   ├── vaca-database.env
│   ├── vaca-localstack.env
│   ├── vaca-orm.env
│   └── vaca-web.env
└── scripts/                    # Scripts auxiliares
    ├── linux/
    ├── mac/
    └── win/
```

## 🔧 Comandos Úteis

### Verificar logs dos serviços
```bash
# Todos os serviços
docker-compose logs -f

# Serviço específico
docker-compose logs -f vaca-api
docker-compose logs -f vaca-omr
docker-compose logs -f vaca-web
```

### Parar os serviços
```bash
docker-compose down
```

### Reconstruir os containers
```bash
docker-compose up --build
```

### Executar comandos dentro dos containers
```bash
# Acessar o container da API
docker-compose exec vaca-api bash

# Acessar o container do OMR
docker-compose exec vaca-omr bash

# Acessar o container do Web
docker-compose exec vaca-web bash
```

## 🐛 Solução de Problemas

### Porta já em uso
Se alguma porta estiver em uso, você pode modificar as portas no arquivo `docker-compose.yml`.

### Problemas de permissão
Se houver problemas de permissão, certifique-se de que o Docker tem as permissões necessárias:
```bash
sudo chown -R $USER:$USER apps/
```

### Reset completo
Para fazer um reset completo (remover volumes e reconstruir):
```bash
docker-compose down -v
docker-compose up --build
```

## 🤝 Contribuição

1. Faça o fork dos repositórios necessários
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença [MIT](LICENSE).
---