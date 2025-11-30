# Chatbot & Automation Infrastructure

Este repositório contém a infraestrutura completa baseada em Docker para rodar um ecossistema de automação incluindo **Supabase**, **Evolution API** (WhatsApp), **n8n** (Workflow Automation) e **Chatwoot** (Atendimento Omnichannel), tudo orquestrado via **Traefik**.

## 📂 Estrutura do Projeto

* **`infra/`**: Configurações do Proxy Reverso (Traefik) e Certificados SSL.
* **`supabase/`**: Stack completa do Supabase Self-hosted.
* **`automation/`**: Aplicações de negócio (n8n, Evolution API, Chatwoot, Redis, Postgres).

## 🚀 Pré-requisitos

* Docker & Docker Compose instalados.
* Um domínio configurado (ex: `seu-dominio.com`) apontando para o IP do servidor.
* Gerar certificados SSL locais ou configurar Let's Encrypt no Traefik.

## 🛠️ Instalação e Configuração

### 1. Configurar Variáveis de Ambiente

Em cada pasta (`infra`, `automation`, `supabase`), renomeie os arquivos `.env.example` para `.env` e edite-os com suas senhas e domínios.

```bash
# Exemplo
cp automation/.env.example automation/.env
nano automation/.env
```
### 2. Certificados SSL

Este projeto espera certificados SSL na pasta infra/certs. Para ambiente de desenvolvimento local, você pode usar o mkcert:

# Na pasta infra/certs

```bash
mkcert -install
mkcert -key-file local-key.pem -cert-file fullchain.pem "seu-dominio.com" "*.seu-dominio.com" "localhost" 127.0.0.1 ::1
```

### 3. Redes Docker

Crie as redes externas necessárias antes de iniciar:

```bash
docker network create traefik-net
docker network create supabase_default
docker network create n8n-net
```

### 4. Inicialização

* Passo 1: Infraestrutura (Traefik)

```bash
cd infra
docker compose up -d
```

* Passo 2: Supabase (Banco de Dados)

```bash
cd ../supabase
docker compose up -d
Aguarde o banco de dados inicializar completamente.
```

* Passo 3: Aplicações (Automação)

```bash
cd ../automation
docker compose up -d
```

Obs: Os containers (chatwoot e n8n) estão sem a labels do traefik porque eles estão sendo acessados fora do ambiente local através do Cloudflare Tunnel, caso o uso seja apenas local, basta adicionar as labels do traefik neles.

## 🤝 Contribuição
Sinta-se à vontade para abrir Issues ou Pull Requests para melhorar esta stack.