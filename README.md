<div align="center">
  <img src="doc/vaca-logo.png" alt="VACA Logo" width="300"/>
</div>

# VACA - Ambiente Virtual de Correção de Avaliação

> **✅ Atualização importante SEMANA 12:** o ambiente completo (API, OMR, Web e infra de apoio) está subindo via `docker compose up -d` sem erros após as últimas correções. Basta seguir o passo a passo abaixo para reproduzir.

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

### 4. Configure o arquivo /etc/hosts

Para que os serviços possam se comunicar corretamente usando os nomes dos containers, você precisa adicionar os seguintes mapeamentos ao arquivo `/etc/hosts`:

#### No Linux/macOS:
```bash
sudo nano /etc/hosts
```

#### No Windows:
Edite o arquivo `C:\Windows\System32\drivers\etc\hosts` como administrador.

Adicione as seguintes linhas ao final do arquivo:

```
# VACA Project
127.0.0.1    vaca-api
127.0.0.1    vaca-omr
127.0.0.1    vaca-web
127.0.0.1    vaca-database
127.0.0.1    vaca-redis
127.0.0.1    vaca-localstack
```

**⚠️ Importante**: Esta configuração é necessária para que as aplicações possam se comunicar entre si usando os nomes dos containers ao invés de `localhost`. Sem essa configuração, as aplicações não conseguirão acessar umas às outras corretamente.

### 5. Execute o projeto

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
| vaca-api | 11000 | http://vaca-api:11000 | API Backend |
| vaca-omr | 11001 | http://vaca-omr:11001 | Serviço de OCR/OMR |
| vaca-web | 11002 | http://vaca-web:11002 | Interface Web |
| PostgreSQL | 5432 | vaca-database:5432 | Banco de dados |
| Redis | 6379 | vaca-redis:6379 | Cache/Session Store |
| LocalStack | 4566 | http://vaca-localstack:4566 | AWS Services Local |

**📝 Nota**: Use os nomes dos containers (vaca-api, vaca-omr, etc.) ao invés de `localhost` nas configurações das aplicações para garantir a comunicação correta entre os serviços.

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

### Problemas de comunicação entre containers
Se os serviços não conseguirem se comunicar entre si, verifique:

1. **Arquivo /etc/hosts configurado**: Certifique-se de que adicionou os mapeamentos dos containers no arquivo `/etc/hosts`
2. **Nomes dos containers nas configurações**: Use `vaca-api`, `vaca-omr`, `vaca-database`, etc. ao invés de `localhost` nos arquivos de configuração das aplicações
3. **Network do Docker**: Todos os containers devem estar na mesma rede (`vaca-network`)

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
