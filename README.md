# 🚀 Déploiement de l’application SeniorLove

Ce document explique étape par étape comment déployer l’application SeniorLove (front-end, back-end et base de données) sur Render.

---

## 🟢 Étape 1 — Créer la base PostgreSQL sur Render

1. Connectez-vous à [https://dashboard.render.com](https://dashboard.render.com).
2. Cliquez sur **“New +”** > **“PostgreSQL”**.
3. Donnez-lui un nom (exemple : `seniorlove-db`).
4. Laissez les options par défaut, puis cliquez sur **Create Database**.
5. Une fois créée, notez bien :
   - L’**External Database URL** (exemple) :
     ```
     postgresql://username:password@host:port/database?sslmode=require
     ```

---

## ⚙️ Étape 2 — Configurer le fichier `.env` du back-end

Créez un fichier `.env` à la racine de votre dossier back avec les informations de Render :

```env
PORT=5432
DB_HOST="dpg-xxx.oregon-postgres.render.com"
DATABASE_URL=postgresql://username:password@host:5432/database?sslmode=require
PGUSER="username"
PGPASSWORD="password"
PGDATABASE="database"
NODE_ENV="production"
JWT_SECRET=monSuperSecretToken123!

