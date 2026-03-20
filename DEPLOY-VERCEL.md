# 🚀 Deploy no Vercel - Guia Passo a Passo

## ✅ O que você precisa:
1. Uma conta no GitHub (grátis)
2. Uma conta na Vercel (grátis)
3. Este projeto no seu computador

---

## 📋 PASSO 1: Preparar o Projeto

### 1.1 Criar arquivo .env
Crie um arquivo chamado `.env` na pasta do projeto com isso:

```env
# Banco de dados MySQL (use os dados da HostGator ou crie um gratuito no PlanetScale)
DATABASE_URL=mysql://usuario:senha@host:3306/nome_do_banco

# Segredo para JWT (pode ser qualquer texto aleatório)
JWT_SECRET=coloque-um-segredo-aqui-aleatorio-123456

# ID do app (pode deixar assim)
VITE_APP_ID=niko-ai-store

# Configurações de OAuth (deixe vazio por enquanto)
OAUTH_SERVER_URL=
OWNER_OPEN_ID=

# Configurações de pagamento (deixe vazio por enquanto)
ATLAS_API_KEY=
DEPIX_WALLET_ADDRESS=

# Analytics (deixe vazio)
VITE_ANALYTICS_ENDPOINT=
VITE_ANALYTICS_WEBSITE_ID=
```

---

## 📋 PASSO 2: Subir para o GitHub

### 2.1 Instalar Git (se não tiver)
Baixe aqui: https://git-scm.com/download/windows

### 2.2 Criar repositório no GitHub
1. Vá em https://github.com
2. Clique no botão verde "New"
3. Nome: `niko-ai-store`
4. Deixe como "Public"
5. Clique "Create repository"

### 2.3 Enviar código (no terminal do projeto)
```bash
git init
git add .
git commit -m "Primeiro commit"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/niko-ai-store.git
git push -u origin main
```

---

## 📋 PASSO 3: Deploy na Vercel

### 3.1 Criar conta
1. Vá em https://vercel.com
2. Clique "Sign Up"
3. Escolha "Continue with GitHub"
4. Autorize o acesso

### 3.2 Importar projeto
1. No dashboard da Vercel, clique "Add New..." → "Project"
2. Encontre "niko-ai-store" na lista
3. Clique "Import"

### 3.3 Configurar deploy
1. **Framework Preset**: Deixe "Other"
2. **Build Command**: `npm run build`
3. **Output Directory**: `dist/public`
4. Clique em "Environment Variables" e adicione TODAS do arquivo .env:
   - DATABASE_URL
   - JWT_SECRET
   - VITE_APP_ID
   - (e todas as outras)

### 3.4 Deploy
Clique "Deploy" e aguarde 2-3 minutos!

---

## 📋 PASSO 4: Configurar Banco de Dados

Opções gratuitas:

### Opção A: PlanetScale (MySQL Gratuito)
1. Vá em https://planetscale.com
2. Crie conta gratuita
3. Crie um banco "niko-ai-store"
4. Copie a URL de conexão
5. Atualize a variável DATABASE_URL na Vercel

### Opção B: Usar banco da HostGator
Use a mesma URL de conexão que criou lá!

---

## ✅ PRONTO!

Seu site estará online em algo como:
`https://niko-ai-store.vercel.app`

---

## 🆘 Precisa de ajuda?
Se travar em algum passo, me fala em qual que eu te ajudo!
