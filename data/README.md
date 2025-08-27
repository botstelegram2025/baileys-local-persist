# Baileys API (Persistência Local)

API simples para WhatsApp via Baileys com **persistência local** (sem banco/API externa). \
Cada sessão de usuário grava seus arquivos em `AUTH_BASE_DIR/auth_info_<sessionId>`.

## Endpoints
- `GET /status/:sessionId`
- `GET /qr/:sessionId`
- `GET /status?sessionId=...` (compat)
- `GET /qr?sessionId=...` (compat)
- `GET /sessions`
- `POST /send-message` (body: `{ number, message, session_id }`)
- `POST /reconnect/:sessionId`
- `POST /clear-session/:sessionId` (apaga credenciais locais!)

## Rodar local (Node 18+)
```bash
npm install
npm start
# acesse: http://localhost:3000/qr?sessionId=user_123
```

> Em dev local, por padrão os arquivos vão para `./auth_storage/`.
> **Nunca** versione esses arquivos (há um `.gitignore`).

## Docker
```bash
docker volume create baileys_data
docker build -t baileys-local:latest .
docker run -d --name baileys \
  -p 3000:3000 \
  -e AUTH_BASE_DIR=/data \
  -v baileys_data:/data \
  baileys-local:latest
```

## Docker Compose
```bash
docker compose up -d
# volume nomeado "baileys_data" será criado e montado em /data
```

## Railway
1. Conecte seu repositório.
2. Em **Variables**, adicione `AUTH_BASE_DIR=/data` e `PORT=3000`.
3. Em **Storage (Volumes)**, **Add Volume** com **Mount Path = /data**.
4. Deploy. Nos logs você verá `📦 AUTH_BASE_DIR: /data`.
5. Pareie em `GET /qr?sessionId=user_123`. Depois do restart, não precisa novo QR.

## Observações
- **Não escale múltiplas réplicas** usando a **mesma sessão** simultaneamente.
- `clear-session` remove o diretório da sessão e força novo pareamento.
- Em caso de desconexão com `loggedOut`, o diretório é removido automaticamente.
