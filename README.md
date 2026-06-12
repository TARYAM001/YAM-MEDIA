# 🌍 FasoNews AI

Plateforme d'actualités sur le Burkina Faso et le monde, alimentée par l'IA (Google Gemini).

## Stack Technique

| Couche | Technologie |
|--------|-------------|
| Frontend | Next.js 14 + TypeScript + Tailwind CSS |
| Backend | Node.js + Express |
| Base de données | PostgreSQL |
| IA | Google Gemini API |
| Déploiement | Railway (backend + DB) + Vercel (frontend) |

## Architecture

```
fasonews-ai/
├── frontend/          # Next.js app
├── backend/           # Express API
├── database/          # Migrations SQL
└── .github/workflows/ # CI/CD
```

## Démarrage rapide

### Prérequis
- Node.js 18+
- PostgreSQL 15+
- Clé API Google Gemini

### Installation

```bash
# Cloner le repo
git clone https://github.com/TON_USERNAME/fasonews-ai.git
cd fasonews-ai

# Backend
cd backend
cp .env.example .env   # Remplir les variables
npm install
npm run dev

# Frontend (dans un autre terminal)
cd frontend
cp .env.example .env.local
npm install
npm run dev
```

## Variables d'environnement

### Backend `.env`
```env
PORT=5000
DATABASE_URL=postgresql://user:password@localhost:5432/fasonews
GEMINI_API_KEY=ta_clé_gemini
JWT_SECRET=un_secret_fort
CORS_ORIGIN=http://localhost:3000
```

### Frontend `.env.local`
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

## Déploiement

### Railway (Backend + PostgreSQL)
1. Créer un projet sur [railway.app](https://railway.app)
2. Ajouter un service PostgreSQL
3. Déployer le dossier `backend/`
4. Configurer les variables d'environnement

### Vercel (Frontend)
1. Importer le repo sur [vercel.com](https://vercel.com)
2. Définir `Root Directory` → `frontend`
3. Ajouter `NEXT_PUBLIC_API_URL` pointant vers Railway

## Fonctionnalités

- [x] Collecte RSS automatique
- [x] Résumé IA avec Gemini
- [x] Génération de titres & hashtags
- [x] API REST complète
- [x] Dashboard administrateur
- [ ] Publication Instagram (Phase 2)
- [ ] Notifications push (Phase 3)
