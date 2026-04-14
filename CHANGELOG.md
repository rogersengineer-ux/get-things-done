# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-04-14

### Added
- Initial MVP release
- Sprint creation, editing, and deletion
- Drag-and-drop sprint reordering
- Duration tracking for each sprint
- Completion checkboxes
- Deferral tracking with visual badges
- Claude AI advisor integration
- Local storage persistence
- Mobile-responsive design
- Motivational quote rotation
- Stats tracking (completed/total, total hours)
- Empty state for first-time users

### Technical
- Single-file HTML architecture
- Vanilla JavaScript (no framework dependencies)
- Direct Anthropic API integration (demo mode)
- Mobile-first CSS design
- Native HTML5 drag-and-drop

### Known Limitations
- API calls made directly from browser (not production-ready)
- No offline support yet
- No data export/import
- No multi-day view
- Alert-based AI responses (will upgrade to modal in future)

### Notes
- Production deployment requires Cloudflare Workers proxy for API security
- localStorage max size is ~5MB (sufficient for thousands of sprints)
- Tested on Chrome 120+, Safari 17+, Firefox 121+
