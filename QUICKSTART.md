# 🚀 FreshCart — Quick Start

## To get MongoDB running, choose one option:

### Option A: MongoDB Atlas (Free, No install required)
1. Go to https://cloud.mongodb.com → Create free M0 cluster
2. Create a user → Whitelist IP 0.0.0.0/0 → Copy connection string
3. Edit `backend/.env`:
   ```
   MONGO_URI=mongodb+srv://USERNAME:PASSWORD@cluster0.xxxxx.mongodb.net/freshcart
   ```

### Option B: Install MongoDB locally (Windows)
1. Download from: https://www.mongodb.com/try/download/community
2. Run installer → check "Install MongoDB as a Service"
3. MongoDB will auto-start on port 27017

## Then run:

```powershell
# Terminal 1 — Backend
cd backend
npm run seed    # Seeds 35+ products + admin accounts
npm run dev     # http://localhost:5000

# Terminal 2 — Frontend
cd frontend
npm run dev     # http://localhost:5173
```

## Demo Accounts (after seeding):
| Role | Email | Password |
|---|---|---|
| Admin | admin@freshcart.com | Admin@123 |
| Delivery | delivery@freshcart.com | Delivery@123 |
| Customer | Register on the site | Any password |
