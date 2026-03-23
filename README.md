# 📰 Blog API fait par NJITCHEU TEUMEBA CONSTY RAPHAEL

API REST complète de gestion d'articles de blog, développée avec **Node.js**, Express et SQLite.
Utilisant la documentation swagger.

---

## 🚀 Installation & Lancement

### Prérequis
- [Node.js](https://nodejs.org) v18 ou supérieur
- npm

### Étapes

```bash
# 1. Cloner le dépôt
[git clone https://github.com/votre-username/blog-api.git](https://github.com/consraphael89-cloud/Blog-Api-node/edit/main/README.md)
cd blog-api

# 2. Installer les dépendances
npm install

# 3. Lancer le serveur
npm start

# (Mode développement avec rechargement automatique)
npm run dev
```

Le serveur démarre sur **http://localhost:3000**

---

## 🌐 URLs disponibles

| URL | Description |
|-----|-------------|
| `http://localhost:3000` | Interface web |
| `http://localhost:3000/api-docs` | Documentation Swagger UI |
| `http://localhost:3000/api/health` | Vérification de l'état de l'API |

---

## 📡 Endpoints de l'API

### Base URL : `/api/articles`

---

### POST `/api/articles` — Créer un article

**Corps de la requête (JSON) :**

```json
{
  "titre": "Introduction à Node.js",
  "contenu": "Node.js est un environnement d'exécution...",
  "auteur": "Jean Dupont",
  "categorie": "Tech",
  "tags": ["nodejs", "javascript", "backend"]
}
```

**Champs obligatoires :** `titre`, `contenu`, `auteur`  
**Champs optionnels :** `categorie` (défaut: "Général"), `tags` (défaut: [])

**Réponse succès (201) :**

```json
{
  "success": true,
  "message": "Article créé avec succès",
  "data": {
    "id": 1,
    "titre": "Introduction à Node.js",
    "auteur": "Jean Dupont",
    "categorie": "Tech",
    "tags": ["nodejs", "javascript", "backend"],
    "date_creation": "2026-03-18T10:00:00.000Z",
    "date_modification": "2026-03-18T10:00:00.000Z"
  }
}
```

---

### GET `/api/articles` — Lister les articles

**Paramètres de requête (tous optionnels) :**

| Paramètre | Type | Description | Exemple |
|-----------|------|-------------|---------|
| `categorie` | string | Filtrer par catégorie | `Tech` |
| `auteur` | string | Filtrer par auteur (partiel) | `Jean` |
| `date` | string | Filtrer par date (YYYY-MM-DD) | `2026-03-18` |
| `limit` | integer | Nombre max de résultats | `10` |
| `offset` | integer | Décalage (pagination) | `20` |

**Exemples :**

```
GET /api/articles
GET /api/articles?categorie=Tech
GET /api/articles?auteur=Jean&date=2026-03-18
GET /api/articles?limit=10&offset=0
```

**Réponse succès (200) :**

```json
{
  "success": true,
  "total": 25,
  "count": 10,
  "data": [ /* tableau d'articles */ ]
}
```

---

### GET `/api/articles/search?query=texte` — Rechercher

Recherche dans le **titre** et le **contenu** des articles.

```
GET /api/articles/search?query=javascript
```

**Réponse succès (200) :**

```json
{
  "success": true,
  "query": "javascript",
  "count": 3,
  "data": [ /* articles correspondants */ ]
}
```

---

### GET `/api/articles/:id` — Récupérer un article

```
GET /api/articles/1
```

**Réponse succès (200) :**

```json
{
  "success": true,
  "data": {
    "id": 1,
    "titre": "...",
    "contenu": "...",
    "auteur": "...",
    "categorie": "Tech",
    "tags": ["nodejs"],
    "date_creation": "2026-03-18T10:00:00.000Z",
    "date_modification": "2026-03-18T10:00:00.000Z"
  }
}
```

**Réponse 404 :**

```json
{
  "success": false,
  "message": "Article avec l'ID 99 non trouvé"
}
```

---

### PUT `/api/articles/:id` — Modifier un article

Seuls les champs envoyés seront modifiés. L'auteur ne peut pas être modifié.

```
PUT /api/articles/1
```

**Corps de la requête :**

```json
{
  "titre": "Nouveau titre",
  "categorie": "Tutoriel",
  "tags": ["updated"]
}
```

**Réponse succès (200) :**

```json
{
  "success": true,
  "message": "Article mis à jour avec succès",
  "data": { /* article mis à jour */ }
}
```

---

### DELETE `/api/articles/:id` — Supprimer un article

```
DELETE /api/articles/1
```

**Réponse succès (200) :**

```json
{
  "success": true,
  "message": "Article \"Introduction à Node.js\" supprimé avec succès",
  "data": { "id": 1, "titre": "Introduction à Node.js" }
}
```

---

## ⚠️ Codes HTTP utilisés

| Code | Signification | Cas d'usage |
|------|--------------|-------------|
| `200` | OK | Lecture, modification, suppression réussie |
| `201` | Created | Création réussie |
| `400` | Bad Request | Données invalides, champs manquants |
| `404` | Not Found | Article introuvable |
| `500` | Internal Server Error | Erreur serveur |

---

## 🏗️ Structure du projet

```
blog-api/
├── src/
│   ├── app.js                    # Point d'entrée Express
│   ├── config/
│   │   ├── database.js           # Configuration SQLite
│   │   └── swagger.js            # Configuration Swagger
│   ├── routes/
│   │   └── article.routes.js     # Définition des routes + JSDoc Swagger
│   ├── controllers/
│   │   └── article.controller.js # Logique métier
│   └── models/
│       └── article.model.js      # Accès base de données
├── public/
│   └── index.html                # Interface web
├── data/
│   └── blog.db                   # Base SQLite (auto-créée)
├── package.json
├── .gitignore
└── README.md
```

---

## 🛠️ Technologies utilisées

- **Node.js** + **Express** — Serveur HTTP
- **better-sqlite3** — Base de données SQLite (synchrone, performant)
- **express-validator** — Validation des entrées
- **swagger-ui-express** + **swagger-jsdoc** — Documentation automatique
- **cors** — Gestion des CORS

---

## 📦 Exemple avec curl

```bash
# Créer un article
curl -X POST http://localhost:3000/api/articles \
  -H "Content-Type: application/json" \
  -d '{"titre":"Test","contenu":"Contenu de test ici","auteur":"Moi","categorie":"Test"}'

# Lister tous les articles
curl http://localhost:3000/api/articles

# Filtrer par catégorie
curl "http://localhost:3000/api/articles?categorie=Tech"

# Rechercher
curl "http://localhost:3000/api/articles/search?query=nodejs"

# Modifier
curl -X PUT http://localhost:3000/api/articles/1 \
  -H "Content-Type: application/json" \
  -d '{"titre":"Titre modifié"}'

# Supprimer
curl -X DELETE http://localhost:3000/api/articles/1
```

