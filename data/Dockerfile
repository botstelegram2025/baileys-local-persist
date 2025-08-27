# Baileys API - persistência local
# Usa Node 20 (tem fetch e bom suporte TLS)
FROM node:20-alpine

# Criar diretórios
WORKDIR /app
RUN mkdir -p /data

# Copiar manifests e instalar deps (cache eficiente)
COPY package*.json ./
RUN npm ci --omit=dev

# Copiar código
COPY server.js ./

# Sugerir volume persistente e variáveis
VOLUME ["/data"]
ENV AUTH_BASE_DIR=/data
ENV PORT=3000

# Rodar como usuário não-root por segurança
USER node

EXPOSE 3000
CMD ["node", "server.js"]
