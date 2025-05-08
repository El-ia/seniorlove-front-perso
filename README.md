# 🛰️ Deployment Documentation – SeniorLove

This document details the necessary steps to deploy the entire SeniorLove project, including:
- A PostgreSQL database hosted on Render
- A Node.js backend deployed on Render
- A static SPA frontend also deployed on Render

## ✅ Step 1 — Creating the PostgreSQL Database on Render

1. Log in to [Render](https://render.com)
2. Click on **"New +"** > **"PostgreSQL"**
3. Name the database (example: `seniorlove-db`)
4. Leave the default options and click on **"Create Database"**
5. Once the database is created, carefully note:
   - **External Database URL** → to be used in your `.env` file

## ⚙️ Step 2 — Initializing the Database from Local Machine

1. In the terminal, navigate to the backend root directory
2. Fill in the `.env` file with the environment variables provided by Render:

```
PORT=5432
DB_HOST=dpg-xxxxxxx.oregon-postgres.render.com
DATABASE_URL=postgresql://<USERNAME>:<PASSWORD>@<HOST>/<DBNAME>?sslmode=require
PGUSER=<USERNAME>
PGPASSWORD=<PASSWORD>
PGDATABASE=<DBNAME>
NODE_ENV=production
JWT_SECRET=<JWT_SECRET>
```

3. Execute the following commands to create and populate the database:

```bash
PGPASSWORD="YOUR_PASSWORD" \
psql -h dpg-xxxxxxx.oregon-postgres.render.com -U YOUR_USERNAME -d YOUR_DBNAME -f ./data/01.create-tables.sql

PGPASSWORD="YOUR_PASSWORD" \
psql -h dpg-xxxxxxx.oregon-postgres.render.com -U YOUR_USERNAME -d YOUR_DBNAME -f ./data/02.seed-tables.sql
```

4. Automate operations via scripts in `package.json`:

```json
{
  "scripts": {
    "db:create": "psql -h dpg-xxxxxx.oregon-postgres.render.com -U <USER> -d <DB> -f './data/01.create-tables.sql'",
    "db:seed": "psql -h dpg-xxxxxx.oregon-postgres.render.com -U <USER> -d <DB> -f './data/02.seed-tables.sql'",
    "db:reset": "npm run db:create && npm run db:seed"
  }
}
```

## 🟡 Step 3 — Deploying the Backend on Render

### 🔧 Web Service Configuration

1. Click on **"New +"** > **"Web Service"**
2. Select **"Deploy from GitHub"** and choose your backend repository
3. Configure the service as follows:
   - **Environment**: Node
   - **Branch**: main
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Root Directory**: `/` (if package.json is at the root)

### 🔐 Environment Variables to Add

Add all the variables defined in your local `.env` file to the **Environment** tab, including:
- `DATABASE_URL`
- `PGUSER`, `PGPASSWORD`, `PGDATABASE`
- `JWT_SECRET`
- `NODE_ENV`

## 🟦 Step 4 — Deploying the Frontend (Static Part)

### 1. Preparation
- The frontend project must be **pushed to GitHub**
- Verify that `npm run build` works locally and generates a `dist` folder with Vite

### 2. Creating the Static Site
- On Render, click on **"New" > "Static Site"**
- Select your `seniorlove-front-perso` repository

### 3. Configuration
- **Build Command**: `npm run build`
- **Publish Directory**: `dist`
- **Root Directory**: `/`

### 4. Environment Variables to Define
Still in **Environment > Add Environment Variable**:
- `VITE_API_URL`: `https://seniorlove-back-perso.onrender.com/api`

In your code, you can access it via:
```javascript
// config.js
export const apiUrl = import.meta.env.VITE_API_URL;
```

### 5. CORS Configuration
Configure CORS in the backend with the correct frontend URL to authorize cross-origin requests.

### 6. Final Deployment
Push all changes to GitHub so that Render triggers a new deployment with the recent additions.
