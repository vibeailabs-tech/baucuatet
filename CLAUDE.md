# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser game: Vietnamese Tết dice gambling game (Bầu Cua Tôm Cá). The entire application lives in `index.html` — no build step, no dependencies, no server required.

## Running the game

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or use a local HTTP server:
```bash
python3 -m http.server 8000
# Then visit http://localhost:8000
```

## Testing changes

1. Edit `index.html` in any text editor
2. Refresh the browser to see changes
3. Use browser DevTools (`F12` / `Cmd+Option+I`) to:
   - Inspect elements and styles
   - Debug JavaScript (set breakpoints in the game flow)
   - Monitor Web Audio API state in the console
4. Test on different screen sizes to verify responsive layout

## Architecture

Everything is contained in `index.html`:

- **CSS** (`<style>` block) — festive red-and-gold Tết theme; responsive grid layout for the 6-cell betting board. Breakpoints scale from mobile (480px max-width) to desktop.
- **HTML** — static structure: balance bar, betting board, chip buttons, action row, dice area, result banner, history panel, game-over modal. Includes semantic ARIA labels for accessibility.
- **JavaScript** (`<script>` block) — all game logic in plain JS, no frameworks

**Key state variables:**
- `balance` — current xu (coin) count, starts at 1000 (resets on restart, no persistence)
- `bets` — object mapping symbol id → bet amount for the current round
- `rolling` — boolean lock preventing input during the dice animation
- `activeChip` — currently selected chip denomination for quick betting
- `audioCtx` — Web Audio API context (lazy-initialized on first music toggle)
- `musicOn` — boolean toggle for background music playback

**Game flow:**
1. Player clicks cells to select symbols (`selectCell`) — toggling cells also refunds their bets
2. Player clicks chip buttons to add bet amounts (`selectChip` → `addBet`) — adds to all selected cells evenly
3. `roll()` runs an 18-frame animation interval (60ms per frame) then calls `settle(results)`
4. `settle()` computes net win/loss: each matching die pays `betAmt × (hits + 1)`; clears bets and cells after each round
5. Game over state triggered when balance ≤ 0; shows modal with restart option

**Music:** Generated entirely via Web Audio API (`scheduleLoop`/`playNote`/`playDrum`) — no audio files. Uses triangle-wave oscillators for melody and noise-buffer bandpass (180 Hz center) for percussion. Music loops indefinitely while enabled. **Important:** Web Audio context must be resumed after user interaction due to browser autoplay policy; this is handled automatically by `getCtx()`.

**Confetti animation:** CSS `@keyframes fall` animation with randomized duration/delay. Programmatically generated on wins via `launchConfetti()` and auto-cleared after 3 seconds to prevent DOM bloat.

**Symbols:** `bau` (🎃 gourd), `cua` (🦀 crab), `tom` (🦐 shrimp), `ca` (🐟 fish), `ga` (🐓 rooster), `nai` (🦌 deer) — defined in the `SYMBOLS` array. To add a new symbol, add an entry to `SYMBOLS` and the 3×2 grid will auto-expand to fit.

**Important implementation details:**
- No state persistence: balance/history resets on page reload or modal restart
- Visibility change handling: Web Audio context auto-suspends/resumes when tab is hidden (preserves browser CPU)
- Bet refund on deselect: `selectCell()` refunds the full bet amount if deselecting an already-bet cell
- History limit: only the last 10 rolls are displayed in the history panel
