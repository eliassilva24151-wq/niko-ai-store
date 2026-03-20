# 🚀 Guia de Deployment - Niko-Ai Store

## 📦 Arquivos Inclusos

- **Frontend**: React 19 + Tailwind 4 + TypeScript
- **Backend**: Express 4 + tRPC 11 + Node.js
- **Database**: MySQL/TiDB (Drizzle ORM)
- **Pagamentos**: API Atlas (Pix integrado)
- **Autenticação**: Manus OAuth
- **Testes**: 16 testes vitest passando

---

## 🔧 Pré-requisitos

1. **Node.js 22+** e **pnpm** instalados
2. **MySQL/TiDB** database (ou usar Railway, Render, etc)
3. **Variáveis de ambiente** configuradas (veja `.env.example`)

---

## 📋 Passo 1: Extrair e Instalar

```bash
# Extrair arquivo
tar -xzf niko-ai-store-complete.tar.gz
cd niko-ai-store

# Instalar dependências
pnpm install

# Gerar migrations do banco
pnpm db:push
```

---

## 🔑 Passo 2: Configurar Variáveis de Ambiente

Criar arquivo `.env` na raiz do projeto:

```env
# Database
DATABASE_URL=mysql://user:password@host:3306/niko_ai_store

# Autenticação
JWT_SECRET=sua-chave-secreta-super-segura-aqui
VITE_APP_ID=seu-app-id-manus
OAUTH_SERVER_URL=https://api.manus.im
VITE_OAUTH_PORTAL_URL=https://portal.manus.im

# Pagamentos (Atlas/Pix)
ATLAS_API_KEY=sua-chave-atlas-aqui
DEPIX_WALLET_ADDRESS=sua-chave-pix-aqui

# URLs
VITE_APP_URL=https://seu-dominio.com
VITE_APP_TITLE=Niko-Ai - Seu Companheiro Inteligente na Estrada
VITE_APP_LOGO=https://seu-cdn.com/logo.png

# APIs Manus (se usar)
BUILT_IN_FORGE_API_URL=https://api.manus.im
BUILT_IN_FORGE_API_KEY=sua-chave-forge-aqui
VITE_FRONTEND_FORGE_API_KEY=sua-chave-frontend-aqui
VITE_FRONTEND_FORGE_API_URL=https://api.manus.im

# Owner Info
OWNER_NAME=Seu Nome
OWNER_OPEN_ID=seu-open-id

# Analytics (opcional)
VITE_ANALYTICS_ENDPOINT=https://analytics.seu-dominio.com
VITE_ANALYTICS_WEBSITE_ID=seu-website-id
```

---

## 🏗️ Passo 3: Build para Produção

```bash
# Build frontend e backend
pnpm build

# Testar antes de publicar
pnpm test

# Verificar se tudo compilou
ls -la dist/
```

---

## 🚀 Passo 4: Deploy na Hostinger

### Opção A: Node.js Hosting (Recomendado)

1. **Fazer upload dos arquivos:**
   - Fazer upload da pasta `niko-ai-store` via SFTP
   - Ou usar Git (push para repositório)

2. **Conectar via SSH:**
```bash
ssh seu-usuario@seu-host.com
cd /home/seu-usuario/niko-ai-store
```

3. **Instalar e rodar:**
```bash
pnpm install --prod
pnpm db:push
pnpm start
```

4. **Configurar PM2 (para manter rodando):**
```bash
npm install -g pm2
pm2 start "pnpm start" --name "niko-ai-store"
pm2 save
pm2 startup
```

5. **Configurar Nginx (reverse proxy):**
```nginx
server {
    listen 80;
    server_name seu-dominio.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

6. **Ativar SSL (Let's Encrypt):**
```bash
sudo certbot certonly --nginx -d seu-dominio.com
```

### Opção B: Docker (Se Hostinger suporta)

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm && pnpm install --prod

COPY . .

RUN pnpm build

EXPOSE 3000

CMD ["pnpm", "start"]
```

Build e push:
```bash
docker build -t seu-usuario/niko-ai-store .
docker push seu-usuario/niko-ai-store
```

---

## 📊 Estrutura de Pastas

```
niko-ai-store/
├── client/              # Frontend React
│   ├── src/
│   │   ├── pages/      # Páginas (Home, Checkout, Admin, etc)
│   │   ├── components/ # Componentes reutilizáveis
│   │   └── lib/        # Utilitários (tRPC client, etc)
│   └── index.html      # HTML principal
├── server/             # Backend Express
│   ├── routers.ts      # Procedures tRPC
│   ├── db.ts           # Query helpers
│   └── _core/          # Framework (OAuth, context, etc)
├── drizzle/            # Schema e migrations
│   ├── schema.ts       # Definição de tabelas
│   └── migrations/     # SQL migrations
├── storage/            # S3 helpers
├── shared/             # Tipos compartilhados
├── package.json        # Dependências
└── tsconfig.json       # Config TypeScript
```

---

## 🔄 Fluxo de Pedidos

1. **Cliente acessa site** → Home.tsx
2. **Clica em "Comprar"** → Checkout.tsx
3. **Preenche dados** → Valida CEP
4. **Confirma pedido** → trpc.order.create
5. **Backend gera Pix** → API Atlas
6. **Cliente escaneia QR** → Paga via Pix
7. **Webhook confirma** → Order status = "paid"
8. **Admin vê pedido** → Admin panel

---

## 💳 Fluxo de Doação

1. **Cliente clica "Apoiar com Pix"** → Donation modal
2. **Digita valor** (R$20-R$10.000)
3. **Clica "Gerar Pix"** → trpc.donation.create
4. **Backend gera Pix** → API Atlas
5. **Cliente escaneia QR** → Paga via Pix
6. **Webhook confirma** → Doação registrada

---

## 🧪 Testes

```bash
# Rodar todos os testes
pnpm test

# Rodar teste específico
pnpm test server/atlas-api.test.ts

# Watch mode
pnpm test --watch
```

---

## 🔐 Segurança

- ✅ JWT para sessões
- ✅ HTTPS obrigatório
- ✅ CORS configurado
- ✅ Validação de entrada (Zod)
- ✅ Rate limiting recomendado
- ✅ Secrets em .env (nunca commit)

---

## 📞 Suporte

- **Documentação tRPC**: https://trpc.io
- **Drizzle ORM**: https://orm.drizzle.team
- **Tailwind CSS**: https://tailwindcss.com
- **Atlas API**: Contatar suporte Atlas

---

## ✅ Checklist Final

- [ ] Variáveis de ambiente configuradas
- [ ] Database criado e migrations rodadas
- [ ] Build sem erros (`pnpm build`)
- [ ] Testes passando (`pnpm test`)
- [ ] SSL/HTTPS ativado
- [ ] Domínio apontando para servidor
- [ ] PM2 ou similar configurado
- [ ] Backups do banco configurados
- [ ] Monitoramento ativado

---

**Pronto para rodar! 🚀**
