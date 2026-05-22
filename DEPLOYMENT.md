# 🚀 FreshCart Deployment Guide

## Local Development

### Requirements
- Node.js ≥ 18.0
- MongoDB ≥ 6.0 (local or Atlas)
- npm ≥ 9.0

### Setup Steps

```bash
# 1. Clone repository
git clone https://github.com/yourusername/freshcart.git
cd freshcart

# 2. Setup backend
cd backend
npm install
cp .env.example .env
# Edit .env: set MONGO_URI and JWT_SECRET

# 3. Seed database
npm run seed

# 4. Start backend
npm run dev

# 5. Setup frontend (new terminal)
cd ../frontend
npm install
npm run dev
```

---

## 🌐 Production Deployment

### Option A: Render.com (Recommended — Free Tier)

#### Backend (Web Service)
1. Go to [render.com](https://render.com) → New → Web Service
2. Connect your GitHub repository
3. Configure:
   - **Root Directory**: `backend`
   - **Build Command**: `npm install`
   - **Start Command**: `node server.js`
4. Add Environment Variables:
   ```
   NODE_ENV=production
   MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/freshcart
   JWT_SECRET=your_super_secret_key_here
   PORT=5000
   ```
5. Deploy → Note the URL (e.g., `https://freshcart-api.onrender.com`)

#### Frontend (Static Site)
1. Go to Render → New → Static Site
2. Configure:
   - **Root Directory**: `frontend`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
3. Add Environment Variable:
   ```
   VITE_API_URL=https://freshcart-api.onrender.com/api
   ```
4. Update `frontend/src/services/api.js`:
   ```js
   baseURL: import.meta.env.VITE_API_URL || 'http://localhost:5000/api'
   ```

---

### Option B: Railway.app

#### Backend
```bash
railway login
railway init
railway add --plugin mongodb
railway up
railway variables set JWT_SECRET=your_secret NODE_ENV=production
```

#### Frontend
```bash
cd frontend
npm run build
# Deploy dist/ folder to Netlify or Vercel
```

---

### Option C: VPS (Ubuntu)

```bash
# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install MongoDB
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
sudo apt-get update && sudo apt-get install -y mongodb-org
sudo systemctl start mongod && sudo systemctl enable mongod

# Install PM2
sudo npm install -g pm2

# Deploy backend
cd /var/www/freshcart/backend
npm install
cp .env.example .env && nano .env   # fill in values
npm run seed
pm2 start server.js --name freshcart-api
pm2 save && pm2 startup

# Build frontend
cd /var/www/freshcart/frontend
npm install && npm run build

# Install Nginx
sudo apt install nginx -y
sudo nano /etc/nginx/sites-available/freshcart
```

**Nginx Configuration:**
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    # Frontend
    location / {
        root /var/www/freshcart/frontend/dist;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    # Backend API proxy
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/freshcart /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx

# SSL with Certbot
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com
```

---

### Option D: MongoDB Atlas (Cloud DB)

1. Go to [cloud.mongodb.com](https://cloud.mongodb.com)
2. Create free M0 cluster
3. Create database user
4. Whitelist IP: `0.0.0.0/0` (or your server IP)
5. Get connection string: `mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/freshcart`
6. Set `MONGO_URI` in your .env

---

## 🔒 Security Checklist

- [ ] Change `JWT_SECRET` to a strong random string (32+ chars)
- [ ] Set `NODE_ENV=production`
- [ ] Use MongoDB Atlas with IP whitelisting
- [ ] Enable HTTPS with SSL certificate
- [ ] Add rate limiting (install `express-rate-limit`)
- [ ] Set CORS to your specific frontend domain only

---

## 📊 Monitoring

```bash
# PM2 monitoring
pm2 status
pm2 logs freshcart-api
pm2 monit

# MongoDB status
sudo systemctl status mongod
```

---

## 🔄 Updates & Redeployment

```bash
cd /var/www/freshcart
git pull origin main

# Backend
cd backend && npm install
pm2 restart freshcart-api

# Frontend
cd ../frontend
npm install && npm run build
```
