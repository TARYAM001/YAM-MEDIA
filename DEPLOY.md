# 🚀 Guide de déploiement – FasoNews AI

## Étape 1 : Préparer GitHub

```bash
# Initialiser le repo
cd fasonews-ai
git init
git add .
git commit -m "feat: initial commit – FasoNews AI"

# Sur GitHub.com : créer un repo public "fasonews-ai"
git remote add origin https://github.com/TON_USERNAME/fasonews-ai.git
git branch -M main
git push -u origin main
```

---

## Étape 2 : Déployer le Backend sur Railway

### 2.1 Créer le projet
1. Aller sur https://railway.app → "New Project"
2. Choisir **"Deploy from GitHub repo"**
3. Sélectionner ton repo `fasonews-ai`
4. Dans **Root Directory** → taper `backend`

### 2.2 Ajouter PostgreSQL
1. Dans Railway → "+ New" → "Database" → **"PostgreSQL"**
2. Railway crée automatiquement `DATABASE_URL`

### 2.3 Variables d'environnement (Settings → Variables)
```
GEMINI_API_KEY     = ta_clé_depuis_aistudio.google.com
JWT_SECRET         = générer_avec: openssl rand -base64 32
CORS_ORIGIN        = https://fasonews.vercel.app  (URL Vercel à mettre après)
NODE_ENV           = production
```

### 2.4 Exécuter la migration
Dans Railway → ton service backend → "Shell" :
```bash
psql $DATABASE_URL -f /app/database/migrations/001_init.sql
```
Ou depuis ton terminal local :
```bash
psql ta_DATABASE_URL_railway -f database/migrations/001_init.sql
```

### 2.5 Récupérer l'URL du backend
Settings → Domains → ex: `https://fasonews-backend.up.railway.app`

---

## Étape 3 : Déployer le Frontend sur Vercel

### 3.1 Importer le projet
1. Aller sur https://vercel.com → "Add New Project"
2. Importer depuis GitHub → `fasonews-ai`
3. **Root Directory** → `frontend`
4. **Framework** → Next.js (auto-détecté)

### 3.2 Variables d'environnement
```
NEXT_PUBLIC_API_URL = https://fasonews-backend.up.railway.app
```

### 3.3 Déployer
Cliquer "Deploy" → attendre 2 minutes → ton site est en ligne !

---

## Étape 4 : Finaliser la configuration

### Mettre à jour CORS_ORIGIN sur Railway
Une fois l'URL Vercel connue (ex: `https://fasonews.vercel.app`),
mettre à jour la variable `CORS_ORIGIN` sur Railway.

### Tester le déploiement
```bash
# Health check backend
curl https://fasonews-backend.up.railway.app/health

# Test login
curl -X POST https://fasonews-backend.up.railway.app/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@fasonews.bf","password":"Admin@123"}'
```

---

## Compte admin par défaut
- **Email** : admin@fasonews.bf
- **Mot de passe** : Admin@123
- ⚠️ **CHANGER CE MOT DE PASSE EN PRODUCTION !**

Pour changer :
```sql
-- Générer un nouveau hash avec bcrypt (rounds=12)
-- Puis mettre à jour :
UPDATE users 
SET password_hash = 'nouveau_hash'
WHERE email = 'admin@fasonews.bf';
```

---

## Flux de travail quotidien

1. Se connecter au dashboard : `/dashboard`
2. Cliquer "Lancer la collecte" → les articles sont collectés et traités par Gemini
3. Onglet "En attente" → Approuver ✅ ou Rejeter ❌ les articles
4. Les articles approuvés apparaissent sur `/news`

---

## Coûts estimés (plan gratuit)

| Service | Plan | Coût |
|---------|------|------|
| Railway | Hobby ($5 crédit/mois) | ~0-5$/mois |
| Vercel | Free tier | Gratuit |
| Gemini API | Free tier (15 req/min) | Gratuit |
| **Total** | | **~0-5$/mois** |
