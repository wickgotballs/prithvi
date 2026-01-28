# 🚀 PRITHVILOK - Render Deployment Guide

## Prerequisites
- GitHub account (to push your code)
- Render account (render.com)
- MongoDB Atlas account (for database)
- Cloudinary account (for image uploads)

---

## Step 1: Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit for Render deployment"
git remote add origin https://github.com/YOUR_USERNAME/prithvi.git
git push -u origin main
```

---

## Step 2: Set Up Render Account

1. Go to [render.com](https://render.com) and sign up
2. Connect your GitHub repository

---

## Step 3: Create Backend Service

1. Click "New +" → "Web Service"
2. Select your GitHub repository
3. Configure:
   - **Name:** `prithvi-backend`
   - **Environment:** Node
   - **Build Command:** `cd backend && npm install`
   - **Start Command:** `cd backend && npm start`
   - **Plan:** Free (or paid if needed)

4. Add Environment Variables:
   ```
   PORT=5000
   MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/dbname
   JWT_SECRET=your_secure_jwt_secret
   JWT_EXPIRES_IN=7d
   CLOUDINARY_NAME=your_cloudinary_name
   CLOUDINARY_KEY=your_cloudinary_api_key
   CLOUDINARY_SECRET=your_cloudinary_secret
   ```

5. Deploy

---

## Step 4: Create Frontend Static Site

1. Click "New +" → "Static Site"
2. Select your GitHub repository
3. Configure:
   - **Name:** `prithvi-frontend`
   - **Build Command:** `cd frontend && npm install && npm run build`
   - **Publish Directory:** `frontend/dist`

4. Deploy

---

## Step 5: Update Frontend API URL

After backend deploys, update the frontend to use the backend API:

1. In `frontend/src/` files, replace API calls from `http://localhost:5000` to your Render backend URL
2. Or set a `.env` file in frontend:
   ```
   VITE_API_URL=https://prithvi-backend.onrender.com
   ```

---

## Step 6: Update Community Link

Update the Community navigation link in `frontend/src/layout/DashboardLayout.jsx`:

```javascript
onClick={() => {
  window.location.href = 'https://your-frontend-url.onrender.com/environment-twitter/login.html';
}}
```

Or deploy environment-twitter separately as another static site on Render.

---

## Step 7: Configure Database

### MongoDB Atlas Setup:
1. Go to [mongodb.com/cloud](https://mongodb.com/cloud)
2. Create a cluster
3. Get connection string: `mongodb+srv://username:password@cluster.mongodb.net/dbname`
4. Add this to Render backend environment variables

---

## Step 8: Troubleshooting

### Backend not connecting to database?
- Check `MONGO_URI` in environment variables
- Ensure IP whitelist includes Render's IPs (0.0.0.0/0)

### Frontend not loading?
- Clear browser cache
- Check build command output in Render logs
- Ensure `frontend/dist` folder exists after build

### CORS errors?
- Update backend CORS settings to include your Render frontend URL:
  ```javascript
  cors({
    origin: ['https://your-frontend-url.onrender.com', 'http://localhost:5174'],
    credentials: true
  })
  ```

---

## Step 9: Monitor & Maintain

- View logs: Dashboard → Service → Logs
- Redeploy: Push to GitHub (auto-deploys)
- Check performance: Dashboard → Metrics

---

## Useful Links
- [Render Docs](https://render.com/docs)
- [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
- [Cloudinary Docs](https://cloudinary.com/documentation)
