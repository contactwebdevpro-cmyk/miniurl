# SnapLink — Déploiement Vercel

## Structure
```
snaplink/
├── index.html          ← Front-end
├── vercel.json         ← Routing
├── package.json        ← Dépendances
└── api/
    ├── shorten.js      ← POST /api/shorten  → crée le lien
    ├── redirect.js     ← GET /:slug         → redirige (avec pubs)
    └── stats.js        ← GET /api/stats     → compteur total
```

## Étapes de déploiement

### 1. Installer Vercel CLI
```bash
npm i -g vercel
```

### 2. Déployer le projet
```bash
cd snaplink
vercel --prod
```

### 3. Activer Vercel KV (base de données)
1. Aller sur https://vercel.com/dashboard
2. Sélectionner ton projet → onglet **Storage**
3. Cliquer **Create Database** → choisir **KV**
4. Nommer la base (ex: `snaplink-kv`) → créer
5. Cliquer **Connect to Project** → sélectionner ton projet

Les variables d'environnement (`KV_URL`, `KV_REST_API_URL`, etc.) sont automatiquement ajoutées.

### 4. Redéployer
```bash
vercel --prod
```

## Comment ça marche

- `POST /api/shorten` : reçoit `{ url }`, génère un slug de 6 chars, stocke dans KV, renvoie `{ slug }`
- `GET /:slug` : récupère l'URL depuis KV, sert une page interstitielle avec les scripts publicitaires pendant 5s, puis redirige
- `GET /api/stats` : renvoie le nombre total de liens créés

## Liens expirés
Les liens expirent après **1 an** (configurable dans `api/shorten.js`, paramètre `ex`).
