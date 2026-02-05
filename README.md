# 🤖 Telegram Clima Bot com N8N

Este projeto implementa um chatbot no Telegram que retorna a temperatura
atual de qualquer cidade:

-   **N8N**
-   **Telegram Bot API** para interação com o usuário
-   **OpenWeather API** para consultar o clima

------------------------------------------------------------------------

## ✅ Funcionalidades

O bot:

-   Recebe uma cidade no formato: `Cidade,UF`
-   Normaliza texto (remove acentos e espaços extras)
-   Consulta a API OpenWeather
-   Retorna a temperatura em °C
-   Trata erros quando a cidade não é encontrada

**Exemplo de uso no Telegram:**

    Salvador,BA

**Resposta esperada:**

    🌤️ A temperatura em Salvador é de 25°C.

------------------------------------------------------------------------

## 🔑 Variáveis de Ambiente (OBRIGATÓRIAS)

Antes de rodar o workflow, configure no N8N:

### 1) OpenWeather

Crie uma variável de ambiente:

    OPENWEATHER_API_KEY= SUA_CHAVE_AQUI

### 2) Telegram

Crie um bot no @BotFather e configure no N8N a credencial:

    TELEGRAM_BOT_TOKEN= SEU_TOKEN_AQUI

------------------------------------------------------------------------

## 📥 Como importar o workflow no N8N

1.  Abra o N8N
2.  Clique em **Import Workflow**
3.  Selecione o arquivo:

```
workflow-chatbot-telegram.json
```
    
4.  Ajuste as credenciais:
    -   Telegram Bot
    -   Variável `OPENWEATHER_API_KEY`
5.  Ative o workflow

------------------------------------------------------------------------

## 🧪 Testes recomendados

Teste com pelo menos 3 cidades:

-   `São Paulo,SP`
-   `Salvador,BA`
-   `Recife,PE`

Teste erro:

-   `CidadeInventada,XX`

Resposta esperada:

    ❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).

------------------------------------------------------------------------

## 🐳 (Opcional) Docker para rodar N8N localmente

``` yaml
services:
  n8n-editor:
    image: n8nio/n8n:2.6.3
    # build: ./
    container_name: n8n-editor-container
    ports:
      - "5678:5678"
    environment:
      - GENERIC_TIMEZONE=America/Sao_Paulo
      - N8N_LOG_LEVEL=info
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
      - N8N_RUNNERS_ENABLED=true
      - DB_SQLITE_POOL_SIZE=1
      - N8N_BLOCK_ENV_ACCESS_IN_NODE=false
      - N8N_GIT_NODE_DISABLE_BARE_REPOS=true 
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
      - OPENWEATHER_API_KEY=${OPENWEATHER_API_KEY}
      - TELEGRAM_BOT_TOKEN=${TELEGRAM_BOT_TOKEN}
    volumes:
      - ./n8n-data:/home/node/.n8n
    networks:
      - n8n-rbmn
    restart: always

networks:
  n8n-rbmn:
    driver: bridge


```

Rodar com:

    docker-compose up -d

Acesse: http://localhost:5678
