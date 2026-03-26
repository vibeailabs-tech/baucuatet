# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser game: Vietnamese Tết dice gambling game (Bầu Cua Tôm Cá). The entire application lives in `index.html` — no build step, no dependencies, no server required.

## Quick Start

**Run the game:**
```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
# Or use a local HTTP server:
python3 -m http.server 8000
```

**Edit & test:**
1. Edit `index.html` in any text editor
2. Refresh browser (Cmd+Shift+R for hard refresh)
3. Use DevTools (F12 / Cmd+Option+I) to debug

## Rules & Guidelines

See `.claude/rules/` for project-specific conventions:

- **[code-style.md](rules/code-style.md)** — HTML/CSS/JS formatting, single-file structure
- **[testing.md](rules/testing.md)** — Manual testing workflow, test checklist, DevTools tips
- **[security.md](rules/security.md)** — XSS prevention, Web Audio API safety, data privacy

## Architecture

Everything is contained in `index.html`:

**CSS** (`<style>` block)
- Festive red-and-gold Tết theme with radial gradients
- Responsive grid layout (3×2 betting board, responsive breakpoints)
- Animations: dice shake, confetti fall, button hover effects

**HTML**
- Semantic structure: balance bar, betting board, chip buttons, action row, dice, result banner, history panel, game-over modal
- ARIA labels for accessibility (keyboard navigation, screen readers)

**JavaScript** (`<script>` block)
- Pure vanilla JS, no frameworks
- Game state: `balance`, `bets`, `rolling`, `activeChip`, `audioCtx`, `musicOn`
- Music via Web Audio API (no audio files): melody + bass + drums, loops indefinitely

## Game Flow

1. Player clicks cells to select symbols (`selectCell`)
2. Player clicks chip buttons to add bets (`selectChip` → `addBet`)
3. Player clicks "Roll" to start dice animation (`roll()` → 18 frames at 60ms each)
4. Dice settle and `settle()` computes payouts: `betAmt × (hits + 1)` per matching die
5. Game over when `balance ≤ 0`; modal allows restart
6. History shows last 10 rolls with payout/dice results

## Key State Variables

- `balance` — current xu (coin) count, starts at 1000
- `bets` — object mapping symbol ID → bet amount for current round
- `rolling` — boolean lock preventing input during dice animation
- `activeChip` — currently selected chip denomination (+10, +50, +100, +500)
- `audioCtx` — Web Audio API context (lazy-initialized)
- `musicOn` — boolean toggle for background music

## Main Game Functions

**Betting:**
- `selectCell(id)` — toggle cell selection; refund bet on deselect
- `selectChip(amount)` — set active denomination and add bet
- `addBet(amount)` — add to all selected cells (validates balance)
- `doubleDown()` — double current bets
- `betAll()` — distribute remaining balance evenly

**Rolling & Settlement:**
- `roll()` — 18-frame animation, then settle with results
- `settle(results)` — compute payouts, update balance, trigger effects
- `updateBalance()` — sync balance display
- `updateBadge(id)` / `removeBetBadge(id)` — render bet amount badges

**Music:**
- `toggleMusic()` — toggle Web Audio playback
- `scheduleLoop()` — schedule next melody + bass + drums loop
- `playNote(ctx, freq, t, dur, vol)` — triangle-wave oscillator
- `playDrum(ctx, t, vol)` — bandpass-filtered white noise percussion

**Utilities:**
- `resetGame()` — reset balance, bets, UI; stop music
- `flash(msg)` — temporary error message
- `launchConfetti()` — 60 pieces, 3-second auto-cleanup

## Important Implementation Details

- **No persistence:** balance/history reset on page reload or modal restart
- **Web Audio context:** Must be resumed after user interaction (browser autoplay policy); handled in `getCtx()`
- **Visibility handling:** Web Audio auto-suspends/resumes when tab is hidden (preserves CPU)
- **Bet refund:** `selectCell()` refunds full bet if deselecting already-selected cell
- **History limit:** Only last 10 rolls displayed
- **Symbols:** `bau`, `cua`, `tom`, `ca`, `ga`, `nai` — defined in `SYMBOLS` array; 3×2 grid auto-expands if symbol count changes

## DOM & CSS Quick Reference

**Key element IDs:**
- `#balance` — balance number (update via textContent)
- `#board` — betting grid (6 cells, auto-populated)
- `#cell-{id}` — betting cells (add/remove `selected` class)
- `#rollBtn` — roll button (disable during rolling)
- `#d0`, `#d1`, `#d2` — dice display
- `#resultBanner` — result message (class: `win`/`lose`/`idle`)
- `#historyList` — history rows
- `#confetti` — confetti container
- `#musicBtn` — music toggle
- `#gameOverModal` — game-over dialog (add/remove `open` class)

**Key CSS classes:**
- `.cell.selected` — selected betting cell
- `.die.rolling` — dice shake animation
- `.result-banner.{win,lose,idle}` — result styling
- `.chip-btn.active` — active chip denomination

## Common Edits

**Change symbols:** Edit `SYMBOLS` array (line 370)

**Change colors:** Edit hex values in `<style>` block (e.g., `#ffd700` gold, `#8b0000` red)

**Change payout rates:** Edit `settle()` function (line 532); currently `betAmt * (hits + 1)`

**Change starting balance:** Update `balance = 1000` (line 379) and in `resetGame()` (line 606)

**Change game-over threshold:** In `settle()` (line 569), change `if (balance <= 0)` condition

## Browser Testing

- **Web Audio API:** Music requires user interaction first (autoplay policy)
- **Responsive:** Test at 480px (mobile), 768px (tablet), 1200px (desktop)
- **Cross-browser:** Chrome, Safari, Firefox all support Web Audio API; test audio quality on each
- **Mobile:** Test on actual devices for touch/tap behavior
