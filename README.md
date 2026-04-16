# Get Things Done

> Mobile-first productivity PWA for night-before commitment planning with AI advisor

## 🎯 What is this?

**Get Things Done** is a productivity tool designed around a single powerful ritual: **planning tomorrow's work the night before.** 

Instead of waking up to decision paralysis, you deliberately choose, order, and time-commit to focused work units ("Sprints") each evening. An embedded AI advisor (powered by Claude) provides calm, grounding support throughout your planning process.

## ✨ Key Features

- **🎯 Sprint-based planning** - Focused work units with duration estimates
- **📋 Drag-and-drop prioritization** - Reorder tasks to reflect true priorities
- **♻️ Smart deferral tracking** - Surfaces tasks that keep getting postponed
- **🤖 AI advisor** - Claude-powered assistant with calm, supportive tone
- **💾 Local storage** - Your data stays on your device
- **📱 Mobile-first** - Designed for phone use
- **⚡ Zero build step** - Single HTML file, deploy anywhere

## 🚀 Quick Start

### Option 1: Open Locally
1. Download `index.html`
2. Open it in your browser
3. Start planning!

### Option 2: GitHub Pages
1. Fork this repository
2. Go to Settings → Pages
3. Select `main` branch as source
4. Your app will be live at `https://[username].github.io/get-things-done`

## 🎨 Core Concepts

### Sprints
A **Sprint** is a single focused work unit with:
- Clear task description
- Time estimate (in hours)
- Completion status
- Deferral count (if postponed)

**Note:** "Sprint" here means a single work session, not the Agile 2-week cycle.

### The Evening Ritual
1. Review incomplete Sprints from today
2. Add tomorrow's Sprints
3. Reorder by true priority (drag & drop)
4. Assign realistic time estimates
5. Sleep well knowing tomorrow is planned

### AI Advisor Behavior
- **Default tone**: Calm and grounding, never pressuring
- **Smart triggers**: 
  - Surfaces conversation after 5+ deferrals
  - Mentions overcommitment at 7+ Sprints (but doesn't block)
- **Context-aware**: Knows your current Sprint list

## 📋 Project Status

**Current Version:** MVP (v1.0)

### ✅ Implemented
- Sprint CRUD operations
- Drag-and-drop reordering
- Duration tracking
- Completion checkboxes
- Deferral system
- Claude AI integration
- Local storage persistence
- Mobile-responsive UI
- Motivational quotes

### 🔮 Roadmap (V2/V3)
- AI persona selection system
- Full Jarvis-like protocol
- Visual timeline view
- Pattern analytics
- Service worker for offline mode
- Cloudflare Workers API proxy

## 🛠️ Technical Details

### Stack
- **Frontend**: Vanilla HTML/CSS/JavaScript (no frameworks)
- **AI**: Anthropic Claude API (Sonnet 4)
- **Storage**: localStorage
- **Deployment**: GitHub Pages ready

### Architecture Decisions
1. **Single-file design** - No build step required, easy to deploy
2. **localStorage persistence** - No backend needed for MVP
3. **Direct API calls** - Production should use Cloudflare Workers proxy
4. **Native drag-and-drop** - No external libraries
5. **Mobile-first CSS** - Responsive from the ground up

### API Configuration

⚠️ **Important**: The current implementation calls the Anthropic API directly from the browser, which is **not secure for production**. 

**For production deployment:**
1. Set up a Cloudflare Workers proxy
2. Store API key in Workers environment variables
3. Update the fetch URL in `index.html` to point to your Worker

Example Worker structure:
```javascript
// Cloudflare Worker (worker.js)
export default {
  async fetch(request, env) {
    const { message } = await request.json();
    
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': env.ANTHROPIC_API_KEY,
        'anthropic-version': '2023-06-01'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        messages: [{ role: 'user', content: message }]
      })
    });
    
    return new Response(await response.text(), {
      headers: { 'Content-Type': 'application/json' }
    });
  }
};
```

## 🤝 Contributing

This is a solo builder project, but suggestions are welcome!

### Branch Structure
- `main` - Production-ready code only
- `feature/*` - New features (e.g., `feature/timeline-view`)
- `fix/*` - Bug fixes (e.g., `fix/drag-drop-mobile`)
- `v*` - Version milestones (e.g., `v1.1`, `v2.0`)

### Development Workflow
1. Create feature branch from `main`
2. Make changes and test locally
3. Merge to `main` when stable
4. Tag releases with version numbers

## 📄 License

MIT License - Do whatever you want with this!

## 🙏 Acknowledgments

- Inspired by the solo builder community
- Quotes from various thinkers and makers

---

**Philosophy:** Perfect is the enemy of shipped. This tool ships.
