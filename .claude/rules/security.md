# Security Requirements

## XSS (Cross-Site Scripting) Prevention

Since the game is entirely client-side with no external data sources, XSS risk is minimal. However:

- **DOM updates:** Use `textContent` (safe) instead of `innerHTML` for user-generated data
  - ✅ `element.textContent = balance` (safe)
  - ❌ `element.innerHTML = '<span>' + balance + '</span>'` (risky)
- **Bet amounts & history:** Only numeric values and emoji (from hardcoded `SYMBOLS`) — no user input
- **No eval():** Never use `eval()`, `Function()`, or `setTimeout(code, delay)`

**Current code is safe:** All dynamic content (balance, dice results, history) is numeric or emoji from `SYMBOLS` array.

## CSRF (Cross-Site Request Forgery)

Not applicable — no backend, no state persistence, no external API calls.

## Data Persistence & Privacy

- **No localStorage/sessionStorage:** Game state is volatile; balance/history reset on page reload
- **No cookies:** Game doesn't use cookies
- **No analytics/tracking:** No external service calls; all code is local
- **No sensitive data:** Starting balance is 1000 xu (not real money)

If persistence is added in the future:
- Use `localStorage` only for non-sensitive game state (balance, settings)
- Never store personally identifiable information (PII)
- Include a clear privacy statement

## Web Audio API Security

- **Autoplay policy:** Web Audio context must be resumed after user interaction — ✅ handled in `getCtx()`
- **No microphone access:** Never request microphone input
- **No speaker bleed:** Audio output is only from generated Web Audio (no external files to download)

## Content Security Policy (CSP)

If deployed to a server with a CSP header:
- Self-generated CSS/JS only (no `<style>` or `<script>` attributes to external sources)
- No external fonts, images, or scripts — game is fully self-contained
- No inline event handlers if strict CSP is enforced — refactor to event listeners if needed

**Current code:** No CSP needed; inline `onclick` attributes are safe for a local/trusted context.

## Third-Party Dependencies

- **Current:** Zero dependencies (no npm, no CDN links, no external libraries)
- **Goal:** Keep it that way — single-file, zero-dependency game is a feature

If dependencies are needed in the future:
- Evaluate **npm audit** before committing
- Pin exact versions in package.json (no `^` or `~`)
- Review security advisories for Web Audio API libraries

## Code Injection via User Interaction

- **Inputs:** Game has no text input fields (only button clicks)
- **Symbols array:** Hardcoded in code; never loaded from external source
- **History display:** Numeric payouts + emoji only; no string concatenation with user data

**Risk: Minimal.** All dynamic display is safe.

## Visibility/Tab Switching

- ✅ Web Audio context properly suspends when tab hidden (`visibilitychange` listener)
- ✅ No background requests or timers that could drain battery
- ✅ Safe for background tabs

## Testing Checklist

- [ ] Console has no JS errors or warnings
- [ ] Opening DevTools does not crash or hang the game
- [ ] Rapidly clicking buttons doesn't corrupt game state
- [ ] Reloading the page during a roll doesn't cause issues (just resets state)
- [ ] No external HTTP/HTTPS requests appear in Network tab
- [ ] Web Audio playback doesn't cause memory leaks (monitor heap in Performance tab)
