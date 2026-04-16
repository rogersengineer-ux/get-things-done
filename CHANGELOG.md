# Changelog

## [1.1.0] - 2026-04-16

### Added
- Notes/details collapsible dropdown per Sprint (persists in localStorage)
- 60-character title limit with live character counter (appears at 40, warns at 50, red at 58)
- Backward-compatible localStorage migration (notes field injected on load)

### Changed
- Anti-procrastination positioning throughout (title, meta, copy, AI system prompt)
- Quote pool updated to gritty builder/hard-work register (12 quotes)
- Footer tagline: "Where hard work gets planned. And done."
- Empty state: "What's the one thing that needs to happen?" / "Add a Sprint and commit to it."
- Delete button: calm gray (no more anxiety-red)
- Header: centered, larger icon, more top padding, quote centered with more breathing room
- Background: living multi-layer animated orb mesh + film grain overlay
- Cards: deeper background, proper drop shadow, clear separation from background
- Sprint card layout: drag handle + checkbox + title always inline on one row
- Drag reordering: only activates from drag handle icon — text inputs allow copy/paste freely
- Drag tactility: scale + rotate + shadow lift effect, haptic feedback (Vibration API)
- Micro-interactions: card slide-in, button lift on hover, input focus border, stats flash, quote fade-in, AI pulse

### Fixed
- Mobile card layout: title no longer stacks below handle/checkbox
- Copy/paste in title and notes inputs no longer triggers card drag
- Sprint card vertical centering

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