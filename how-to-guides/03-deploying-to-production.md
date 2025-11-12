# How to Deploy Kixx to Production

Guide for deploying Kixx applications to production hosting environments.

## Prerequisites

- Git repository with your Kixx app
- Node.js application (Kixx requires Node.js runtime)
- Choose a hosting provider that supports Node.js

## Hosting Options

### ❌ Won't Work: Shared Hosting

Traditional shared hosting (cPanel, etc.) **will not work** for Kixx applications because:
- Kixx is a Node.js server application, not PHP
- Requires persistent Node.js process
- Needs ability to listen on custom ports

### ✅ Will Work: VPS or PaaS Providers

**PaaS (Platform as a Service)** - Easiest option:
- Railway (recommended for simplicity)
- Render
- Heroku
- Fly.io

**VPS (Virtual Private Server)** - More control:
- DigitalOcean
- Linode
- AWS EC2
- Google Cloud Compute Engine

---

## Deploying to Railway

Railway is recommended for its simplicity and automatic deployments from GitHub.

### 1. Required Configuration Files

**package.json** - Add start script:
```json
{
  "scripts": {
    "start": "kixx app-server --environment production",
    "dev": "kixx app-server --environment development"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

**kixx.config.jsonc** - Configure PORT environment variable:
```jsonc
{
    "server": {
        // Railway provides PORT env variable
        "port": "${PORT:3000}",
        "host": "0.0.0.0"  // Required for Railway
    },
    "environments": {
        "production": {
            "server": {
                "port": "${PORT:3000}",
                "host": "0.0.0.0"
            }
        },
        "development": {
            "server": {
                "port": "3001",
                "host": "localhost"
            }
        }
    }
}
```

**Why `host: "0.0.0.0"`?**
- Railway requires binding to `0.0.0.0` (not `localhost`) to accept external connections
- In development, use `localhost` for security

### 2. Deploy to Railway

1. Push your code to GitHub
2. Go to [railway.app](https://railway.app)
3. Click "New Project" → "Deploy from GitHub repo"
4. Select your repository
5. Railway will automatically:
   - Detect `package.json`
   - Run `npm install`
   - Run `npm start`
   - Provide a `PORT` environment variable
   - Generate a public URL

### 3. Custom Domain Setup

**In Railway:**
1. Go to your service → Settings → Networking
2. Click "+ Custom Domain"
3. Enter your domain (e.g., `example.com`)
4. Railway will provide DNS records

**In your DNS provider (Porkbun, Cloudflare, etc.):**

Add the DNS records Railway provides:

**For root domain (`example.com`):**
```
Type: A (or ALIAS/ANAME if supported)
Host: @
Value: [IP address from Railway]
TTL: 600
```

**For www subdomain (`www.example.com`):**
```
Type: CNAME
Host: www
Value: [Railway domain from settings]
TTL: 600
```

**SSL Certificate:**
- Railway automatically provisions SSL certificates via Let's Encrypt
- Wait 5-15 minutes for certificate to be issued
- Site will be accessible via HTTPS once certificate is ready

### 4. Verify Deployment

```bash
# Test the deployed site
curl -I https://your-domain.com

# Should return:
# HTTP/2 200
```

---

## Deploying to Render

Similar to Railway with a few differences:

### Required Files

Same `package.json` and `kixx.config.jsonc` as Railway.

### Deploy Steps

1. Push code to GitHub
2. Go to [render.com](https://render.com)
3. Click "New" → "Web Service"
4. Connect your GitHub repository
5. Configure:
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Environment**: Node
6. Click "Create Web Service"

### Environment Variables

Render provides `PORT` automatically. No additional configuration needed.

---

## Deploying to VPS (DigitalOcean, Linode, etc.)

### 1. Server Setup

```bash
# SSH into your server
ssh user@your-server-ip

# Install Node.js (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2 for process management
sudo npm install -g pm2
```

### 2. Deploy Application

```bash
# Clone your repository
git clone https://github.com/yourusername/your-repo.git
cd your-repo

# Install dependencies
npm install

# Start with PM2
pm2 start npm --name "my-app" -- start

# Make PM2 start on system boot
pm2 startup
pm2 save
```

### 3. Configure nginx Reverse Proxy

```nginx
# /etc/nginx/sites-available/your-domain

server {
    listen 80;
    server_name your-domain.com www.your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```bash
# Enable site and restart nginx
sudo ln -s /etc/nginx/sites-available/your-domain /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### 4. Setup SSL with Certbot

```bash
# Install Certbot
sudo apt-get install certbot python3-certbot-nginx

# Get SSL certificate
sudo certbot --nginx -d your-domain.com -d www.your-domain.com

# Auto-renewal is configured automatically
```

---

## Environment Variables

### Production Environment Variables

Set these in your hosting provider's dashboard:

- `NODE_ENV=production` (optional, some hosts set this automatically)
- `PORT` (usually provided by host)

### Custom Environment Variables

If your app needs custom env vars:

**Railway/Render:**
- Add in dashboard under "Environment Variables"

**VPS:**
- Create `.env` file (make sure it's in `.gitignore`)
- Or add to PM2 ecosystem file

---

## Deployment Checklist

- [ ] `package.json` has `"start"` script
- [ ] `kixx.config.jsonc` uses `PORT` environment variable
- [ ] `kixx.config.jsonc` sets `host: "0.0.0.0"` for production
- [ ] `.gitignore` excludes sensitive files (`.env`, `node_modules`)
- [ ] Code pushed to GitHub
- [ ] Hosting provider connected to repository
- [ ] Environment variables configured (if needed)
- [ ] Custom domain DNS configured (if using)
- [ ] SSL certificate issued (wait 5-15 min on PaaS)
- [ ] Site accessible via HTTPS

---

## Troubleshooting

### Site Not Loading

**Check server logs:**
- Railway: Click on your service → "Logs" tab
- Render: Click on your service → "Logs" tab
- VPS: `pm2 logs my-app`

**Common issues:**
1. **Wrong `host` setting**: Must be `0.0.0.0` for Railway/Render
2. **Port binding error**: Make sure `PORT` env var is being used
3. **Build failed**: Check that `npm install` succeeded

### SSL Certificate Not Working

**Wait longer:**
- Let's Encrypt provisioning takes 5-15 minutes
- Railway/Render handle this automatically

**Check domain DNS:**
```bash
# Verify DNS is pointing to correct server
dig your-domain.com

# Should show the hosting provider's IP
```

**Force HTTPS:**
Most PaaS providers auto-redirect HTTP → HTTPS once cert is issued.

---

## Continuous Deployment

### Automatic Deployments

**Railway and Render** automatically redeploy when you push to GitHub:

```bash
git add .
git commit -m "Update site content"
git push origin main

# Your site will redeploy automatically!
```

### Manual Deployment (VPS)

Create a deployment script:

```bash
#!/bin/bash
# deploy.sh

cd /path/to/your-repo
git pull origin main
npm install
pm2 restart my-app
```

Run after pushing changes:
```bash
ssh user@server 'bash /path/to/deploy.sh'
```

---

## Next Steps

- Set up monitoring (Railway/Render have built-in metrics)
- Configure custom error pages
- Set up logging and analytics
- Consider CDN for static assets (if needed)
