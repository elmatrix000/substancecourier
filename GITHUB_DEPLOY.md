# GitHub Pages Deployment Guide

## 🚀 Quick Deploy Instructions

### 1. Create GitHub Repository
1. Go to [GitHub](https://github.com) and create a new repository
2. Name it `substancecourier` (or any name you prefer)
3. Make it **Public** (required for free GitHub Pages)

### 2. Push Code to GitHub

```bash
# Navigate to project folder
cd /mnt/okcomputer/output/app

# Initialize git (if not already)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit - Substance Courier website"

# Add remote (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/substancecourier.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### 3. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** tab
3. Scroll down to **Pages** section (or click "Pages" in left sidebar)
4. Under **Source**, select **Deploy from a branch**
5. Select **main** branch and **/ (root)** folder
6. Click **Save**

### 4. Build and Deploy

#### Option A: Manual Deploy (Build locally)

```bash
# Build the project
npm run build

# Create gh-pages branch with dist folder
git add dist -f
git commit -m "Deploy to GitHub Pages"
git subtree push --prefix dist origin gh-pages
```

#### Option B: GitHub Actions (Auto-deploy) ⭐ RECOMMENDED

1. Create `.github/workflows/deploy.yml` file:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout
      uses: actions/checkout@v3
      
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '20'
        
    - name: Install dependencies
      run: npm ci
      
    - name: Build
      run: npm run build
      
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      if: github.ref == 'refs/heads/main'
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

2. Push this file to GitHub - it will auto-deploy on every push!

### 5. Update Vite Config (If needed)

If your repo name is different, update `vite.config.ts`:

```typescript
export default defineConfig({
  base: '/YOUR_REPO_NAME/',  // Change this to your repo name
  // ... rest of config
})
```

For custom domain, use:
```typescript
base: '/',  // For custom domain
```

---

## 📁 Important Files for GitHub Pages

| File | Purpose |
|------|---------|
| `public/404.html` | Handles SPA routing on GitHub Pages |
| `index.html` | Main entry with SPA routing script |
| `vite.config.ts` | Build configuration with base URL |

---

## 🔧 Configuration Settings

### Custom Domain (Optional)

1. Add a `CNAME` file in the `public` folder:
```
www.yourdomain.com
```

2. Configure DNS with your domain provider:
   - Add A record pointing to GitHub Pages IPs
   - Or add CNAME record pointing to `YOUR_USERNAME.github.io`

### Environment Variables

Create `.env` file for different environments:

```env
# .env.production
VITE_APP_URL=https://substancecourier.github.io
VITE_API_URL=https://your-api.com
```

---

## ✅ Pre-Deployment Checklist

- [ ] All images are in `public/images/` folder
- [ ] `vite.config.ts` has correct `base` URL
- [ ] `404.html` exists in `public` folder
- [ ] Build succeeds: `npm run build`
- [ ] No console errors in production build
- [ ] All links use relative paths

---

## 🐛 Troubleshooting

### "Page Not Found" errors
- Make sure `404.html` is in the `public` folder
- Check that `base` in `vite.config.ts` matches your repo name

### Assets not loading
- Use relative paths: `./images/logo.jpg` not `/images/logo.jpg`
- Check browser console for 404 errors

### Routing not working
- Ensure the SPA routing script is in `index.html`
- Try accessing with hash: `/#/track` instead of `/track`

### Build fails
- Run `npm install` to ensure all dependencies
- Check for TypeScript errors: `npx tsc --noEmit`

---

## 📞 Contact Info Configured

| Platform | Value |
|----------|-------|
| WhatsApp | 070 445 7769 |
| Phone | 070 445 7769 |
| Email | substancecourier@gmail.com |

To update: Edit `src/data/store.ts` → `CONTACT_INFO`

---

## 🚴 Features

- ✅ Order via WhatsApp (auto-sends)
- ✅ Order via Email (auto-sends)
- ✅ Live order tracking
- ✅ Share tracking link
- ✅ Area-based pricing
- ✅ Partner restaurants
- ✅ Bicycles only (Motorcycles & Cars coming soon!)

---

**Your site will be live at:** `https://YOUR_USERNAME.github.io/substancecourier/`
