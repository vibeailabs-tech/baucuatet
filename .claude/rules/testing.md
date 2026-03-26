# Testing Conventions

## Manual Testing Workflow

Since there's no automated test suite, all testing is manual via the browser:

1. **Open the game:** `open index.html` (macOS) or use a local HTTP server
2. **Refresh after each edit:** Cmd+Shift+R (hard refresh) to bypass browser cache
3. **Use DevTools:** F12 or Cmd+Option+I to inspect elements, debug JS, monitor console errors

## Test Checklist

### Core Gameplay
- [ ] Click cells to select/deselect (should toggle `.selected` class)
- [ ] Click chip buttons (+10, +50, +100, +500) to add bets
- [ ] Deselecting a cell refunds its full bet amount
- [ ] "Clear bets" button refunds all bets
- [ ] "Double" button doubles current bets (fails if insufficient balance)
- [ ] "All-in" distributes remaining balance evenly across selected cells
- [ ] "Roll" button triggers 18-frame dice animation then settlement

### Settlement & Payouts
- [ ] Winning roll (1, 2, or 3 matching dice): payout = `bet × (hits + 1)`
- [ ] Losing roll (0 matches): bet is lost
- [ ] Balance updates correctly after each round
- [ ] Result banner shows correct win/loss message and amount
- [ ] Confetti launches on win (60 pieces, 3-second auto-cleanup)

### Game Over
- [ ] Game-over modal appears when balance ≤ 0
- [ ] "Restart" button resets balance to 1000, clears history and board
- [ ] "Dismiss" button closes modal without restarting

### History
- [ ] Last 10 rolls appear in history panel with dice and payout
- [ ] History order is newest-first
- [ ] History clears on game restart

### Music
- [ ] Music button initializes Web Audio context on first click
- [ ] Music plays a looping melody with drums and bass
- [ ] Music icon updates: 🔇 (off) → 🔊 (on)
- [ ] Music stops when button clicked again
- [ ] Music auto-pauses when tab becomes hidden (visibility API)
- [ ] Music resumes when tab becomes visible again

### Responsive Layout
- [ ] Mobile (480px): board, controls, dice stack vertically; buttons wrap
- [ ] Tablet (768px): board and controls fit side-by-side or below
- [ ] Desktop (1200px+): clean centered layout with adequate spacing
- [ ] Tap targets are ≥44px (WCAG accessible)

### Accessibility
- [ ] Keyboard navigation: Tab to cycle buttons, Enter/Space to activate
- [ ] Cells have ARIA labels (`aria-label="Bầu"`)
- [ ] Result banner updates with `aria-live="polite"`
- [ ] Modal has `aria-modal="true"` and `role="dialog"`

### Edge Cases
- [ ] Betting without selecting cells: shows flash message "Hãy chọn ô trước!"
- [ ] Betting with insufficient balance: shows "Không đủ xu!"
- [ ] No active chip selected: first click selects chip (defaults to +10)
- [ ] Rolling button disabled during dice animation
- [ ] Selecting/betting disabled during rolling state

## DevTools Tips

- **Console:** Watch for JS errors (should be none)
- **Network:** Confirm single index.html load (no external dependencies)
- **Performance:** Monitor Web Audio API — music should not block main thread
- **Elements:** Inspect `.cell.selected`, `.result-banner.win`, dice animation states
- **Device Emulation:** Test at 375px (iPhone SE), 768px (iPad), 1024px (desktop)

## Browser Compatibility

- Chrome/Edge: Full support (Web Audio API, CSS Grid, modern JS)
- Safari: Full support (test audio context resume behavior)
- Firefox: Full support
- Mobile browsers: Test on actual devices (tap/touch behavior may differ from click)
