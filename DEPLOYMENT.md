# Deployment Guide

## GitHub Pages Deployment

### Initial Setup (One-time)

1. **Create the repository** (if not already done):
   ```bash
   # Navigate to your local project folder
   cd /path/to/get-things-done
   
   # Initialize git
   git init
   
   # Add all files
   git add .
   
   # Initial commit
   git commit -m "Initial commit: Get Things Done MVP v1.0"
   
   # Add remote (replace YOUR_USERNAME with your GitHub username)
   git remote add origin https://github.com/YOUR_USERNAME/get-things-done.git
   
   # Push to GitHub
   git push -u origin main
   ```

2. **Enable GitHub Pages**:
   - Go to your repository on GitHub
   - Click **Settings** → **Pages**
   - Under "Source", select **main** branch
   - Click **Save**
   - Your site will be live at: `https://YOUR_USERNAME.github.io/get-things-done`

### Updating Your Deployment

```bash
# Make your changes to index.html or other files

# Stage changes
git add .

# Commit with descriptive message
git commit -m "Add feature: improved drag-and-drop on mobile"

# Push to GitHub
git push origin main
```

GitHub Pages will automatically rebuild and deploy within 1-2 minutes.

---

## Cloudflare Workers Proxy (Production AI Setup)

⚠️ **Required for production** - Don't expose your API key in the browser!

### Step 1: Create Cloudflare Worker

1. Sign up at [cloudflare.com](https://cloudflare.com) (free tier is fine)
2. Go to **Workers & Pages** → **Create Worker**
3. Name it: `gtd-ai-proxy`
4. Replace the default code with:

```javascript
// worker.js
export default {
  async fetch(request, env) {
    // CORS headers for browser requests
    const corsHeaders = {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'POST, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type',
    };
    
    // Handle preflight
    if (request.method === 'OPTIONS') {
      return new Response(null, { headers: corsHeaders });
    }
    
    // Only allow POST
    if (request.method !== 'POST') {
      return new Response('Method not allowed', { status: 405 });
    }
    
    try {
      const body = await request.json();
      
      // Call Anthropic API
      const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': env.ANTHROPIC_API_KEY,
          'anthropic-version': '2023-06-01'
        },
        body: JSON.stringify({
          model: body.model || 'claude-sonnet-4-20250514',
          max_tokens: body.max_tokens || 1000,
          system: body.system,
          messages: body.messages
        })
      });
      
      const data = await response.json();
      
      return new Response(JSON.stringify(data), {
        headers: {
          'Content-Type': 'application/json',
          ...corsHeaders
        }
      });
      
    } catch (error) {
      return new Response(JSON.stringify({ 
        error: error.message 
      }), {
        status: 500,
        headers: {
          'Content-Type': 'application/json',
          ...corsHeaders
        }
      });
    }
  }
};
```

5. Click **Save and Deploy**

### Step 2: Add API Key to Worker

1. In your Worker dashboard, click **Settings** → **Variables**
2. Under **Environment Variables**, click **Add variable**
3. Name: `ANTHROPIC_API_KEY`
4. Value: Your Anthropic API key (get from console.anthropic.com)
5. Click **Encrypt** (important!)
6. Click **Save**

### Step 3: Update index.html

In your local `index.html`, find the `callClaudeAPI` function and replace:

```javascript
// OLD (line ~450-ish)
const response = await fetch("https://api.anthropic.com/v1/messages", {

// NEW
const response = await fetch("https://gtd-ai-proxy.YOUR_SUBDOMAIN.workers.dev", {
```

Replace `YOUR_SUBDOMAIN` with your Cloudflare Workers subdomain.

### Step 4: Deploy Updated Code

```bash
git add index.html
git commit -m "Update API endpoint to use Cloudflare Workers proxy"
git push origin main
```

### Testing Your Worker

```bash
# Test with curl
curl -X POST https://gtd-ai-proxy.YOUR_SUBDOMAIN.workers.dev \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-sonnet-4-20250514",
    "max_tokens": 100,
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

You should get a JSON response with Claude's message.

---

## Custom Domain (Optional)

### GitHub Pages Custom Domain

1. Buy a domain (e.g., from Cloudflare, Namecheap)
2. In your repo: **Settings** → **Pages** → **Custom domain**
3. Enter your domain (e.g., `getthingsdone.app`)
4. Add DNS records at your registrar:
   ```
   Type: CNAME
   Name: www
   Value: YOUR_USERNAME.github.io
   ```
5. Wait for DNS propagation (5-30 minutes)
6. Enable "Enforce HTTPS" in GitHub Pages settings

---

## Branch Workflow (Feature Development)

### Creating a New Feature

```bash
# Create and switch to feature branch
git checkout -b feature/timeline-view

# Make changes...
# Test locally by opening index.html

# Commit changes
git add .
git commit -m "Add timeline visualization for weekly view"

# Push feature branch
git push origin feature/timeline-view
```

### Merging to Main

```bash
# Switch to main
git checkout main

# Pull latest changes
git pull origin main

# Merge feature
git merge feature/timeline-view

# Push to production
git push origin main

# Delete feature branch (optional)
git branch -d feature/timeline-view
git push origin --delete feature/timeline-view
```

### Quick Hotfix

```bash
# Create hotfix branch
git checkout -b fix/mobile-drag-drop

# Fix the bug...
# Test...

# Commit and push directly to main
git checkout main
git merge fix/mobile-drag-drop
git push origin main
```

---

## Monitoring & Debugging

### GitHub Pages Build Status
- Check: `https://github.com/YOUR_USERNAME/get-things-done/deployments`
- Look for green checkmarks

### Cloudflare Worker Logs
- Go to your Worker → **Logs** tab
- Click **Begin log stream**
- Trigger API calls from your app
- Watch for errors in real-time

### Browser Console Debugging
- Open DevTools (F12)
- Check Console tab for JavaScript errors
- Check Network tab for failed API calls
- Check Application → Local Storage for data

---

## Backup & Recovery

### Export Local Storage Data

Run in browser console:
```javascript
// Export sprints
const sprints = localStorage.getItem('gtd_sprints');
console.log(sprints);
// Copy and save to a file
```

### Import Local Storage Data

```javascript
// Import sprints
const backup = '[YOUR_JSON_BACKUP]';
localStorage.setItem('gtd_sprints', backup);
location.reload();
```

---

## Cost Estimate

**GitHub Pages**: Free
**Cloudflare Workers**: Free tier includes:
- 100,000 requests/day
- 10ms CPU time per request

**Anthropic API** (as of April 2026):
- Sonnet 4: ~$3 per 1M input tokens
- Typical usage: ~500 tokens per AI interaction
- Free tier: $5 credit

**Estimated monthly cost** for solo user: $0-2/month
