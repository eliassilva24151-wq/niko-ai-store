# 🚀 Niko-Ai Store - Setup Completo para Hostinger

## 📦 O que você recebeu

- **niko-ai-store-complete.tar.gz** - Projeto completo pronto para rodar
- **DEPLOYMENT_GUIDE.md** - Guia detalhado de deployment
- **Todos os arquivos** - Frontend, Backend, Database, API de Pagamento

---

## ⚡ Quick Start (5 minutos)

### 1. Extrair o arquivo
```bash
tar -xzf niko-ai-store-complete.tar.gz
cd niko-ai-store
```

### 2. Instalar dependências
```bash
pnpm install
```

### 3. Configurar banco de dados
Editar `.env` com suas credenciais MySQL:
```env
DATABASE_URL=mysql://user:senha@host:3306/niko_ai_store
```

### 4. Rodar migrations
```bash
pnpm db:push
```

### 5. Iniciar servidor
```bash
pnpm start
```

Pronto! Acesse `http://localhost:3000`

---

## 🔑 Variáveis de Ambiente Obrigatórias

Copiar `.env.example` para `.env` e preencher:

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `DATABASE_URL` | Conexão MySQL | `mysql://user:pass@host/db` |
| `JWT_SECRET` | Chave de sessão | Qualquer string aleatória |
| `ATLAS_API_KEY` | Chave API Pix | De seu painel Atlas |
| `DEPIX_WALLET_ADDRESS` | Chave Pix | Sua chave Pix |
| `VITE_APP_TITLE` | Nome do site | Niko-Ai Store |
| `VITE_APP_LOGO` | URL do logo | https://cdn.../logo.png |

---

## 📁 Estrutura do Projeto

```
niko-ai-store/
├── client/              # React Frontend
│   ├── src/pages/       # Páginas (Home, Checkout, Admin, Donation)
│   ├── src/components/  # Componentes reutilizáveis
│   └── src/lib/trpc.ts  # Cliente tRPC
├── server/              # Express Backend
│   ├── routers.ts       # Procedures tRPC (order, donation, admin, etc)
│   ├── db.ts            # Query helpers
│   └── _core/           # Framework (OAuth, context, LLM, etc)
├── drizzle/             # Banco de dados
│   ├── schema.ts        # Tabelas (orders, reservations, etc)
│   └── migrations/      # SQL migrations
├── package.json         # Dependências
└── tsconfig.json        # Config TypeScript
```

---

## 🛍️ Funcionalidades Incluídas

✅ **Loja de Produtos**
- Catálogo com cores personalizáveis
- Carrinho de compras
- Checkout com CEP
- Cálculo de frete automático

✅ **Pagamentos**
- Integração Pix (API Atlas)
- QR Code dinâmico
- Webhook de confirmação
- Status de pedido em tempo real

✅ **Doações**
- Botão "Apoiar com Pix" no footer
- Valores de R$20 a R$10.000
- Doação 100% anônima
- Pix gerado automaticamente

✅ **Admin Panel**
- Acesso com senha (Skate@71)
- Ver todos os pedidos
- Atualizar status de pedidos
- Gerenciar reservas

✅ **Autenticação**
- Manus OAuth integrado
- Sessões com JWT
- Login/Logout automático

✅ **Testes**
- 16 testes vitest passando
- Cobertura de APIs críticas
- Validação de pagamentos

---

## 🚀 Deploy na Hostinger

### Passo 1: Fazer Upload

**Via SFTP:**
```bash
sftp seu-usuario@seu-host.com
put -r niko-ai-store /home/seu-usuario/
```

**Ou via Git:**
```bash
git init
git remote add origin https://seu-repo.git
git push -u origin main
```

### Passo 2: Conectar via SSH

```bash
ssh seu-usuario@seu-host.com
cd /home/seu-usuario/niko-ai-store
```

### Passo 3: Instalar

```bash
pnpm install --prod
pnpm db:push
```

### Passo 4: Rodar com PM2

```bash
npm install -g pm2
pm2 start "pnpm start" --name "niko-ai-store"
pm2 save
pm2 startup
```

### Passo 5: Configurar Nginx

Criar arquivo `/etc/nginx/sites-available/seu-dominio.com`:

```nginx
server {
    listen 80;
    server_name seu-dominio.com www.seu-dominio.com;

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

Ativar:
```bash
sudo ln -s /etc/nginx/sites-available/seu-dominio.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### Passo 6: SSL/HTTPS

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot certonly --nginx -d seu-dominio.com
```

---

## 📊 Banco de Dados

### Tabelas Criadas

- **orders** - Pedidos dos clientes
- **reservations** - Reservas de interesse
- **users** - Usuários do sistema
- **customers** - Dados dos clientes

### Rodar Migrations

```bash
pnpm db:push
```

### Ver dados no banco

```bash
pnpm db:studio
```

---

## 🧪 Testes

```bash
# Rodar todos
pnpm test

# Modo watch
pnpm test --watch

# Teste específico
pnpm test server/atlas-api.test.ts
```

---

## 🔧 Troubleshooting

### Erro: "Cannot find module 'trpc'"
```bash
pnpm install
```

### Erro: "Database connection failed"
- Verificar `DATABASE_URL` em `.env`
- Verificar se MySQL está rodando
- Verificar credenciais

### Erro: "Pix não gera"
- Verificar `ATLAS_API_KEY` em `.env`
- Verificar `DEPIX_WALLET_ADDRESS`
- Testar API com: `pnpm test server/atlas-api.test.ts`

### Site não carrega
- Verificar se servidor está rodando: `pm2 status`
- Verificar logs: `pm2 logs niko-ai-store`
- Verificar Nginx: `sudo nginx -t`

---

## 📞 Suporte Técnico

### Documentação
- **tRPC**: https://trpc.io
- **Drizzle ORM**: https://orm.drizzle.team
- **Tailwind**: https://tailwindcss.com
- **Express**: https://expressjs.com

### Comandos Úteis

```bash
# Ver logs em tempo real
pm2 logs niko-ai-store

# Reiniciar servidor
pm2 restart niko-ai-store

# Parar servidor
pm2 stop niko-ai-store

# Build para produção
pnpm build

# Verificar saúde do servidor
curl http://localhost:3000/api/health
```

---

## ✅ Checklist Final

- [ ] Arquivo extraído
- [ ] `pnpm install` rodou com sucesso
- [ ] `.env` configurado com credenciais
- [ ] `pnpm db:push` rodou sem erros
- [ ] `pnpm test` - 16 testes passando
- [ ] `pnpm start` - servidor rodando em http://localhost:3000
- [ ] Botão "Comprar" funciona
- [ ] Botão "Apoiar com Pix" funciona
- [ ] Admin panel acessível (3 cliques no ©)
- [ ] Pix gera sem erros

---

## 🎉 Pronto para Rodar!

Seu site está 100% pronto. Agora é só fazer upload para Hostinger e configurar o domínio!

**Dúvidas? Consulte DEPLOYMENT_GUIDE.md para instruções detalhadas.**
