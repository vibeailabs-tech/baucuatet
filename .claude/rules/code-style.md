# Code Style Guidelines

## HTML

- Use semantic HTML5 elements where appropriate (`<button>`, `<section>`, `<main>`)
- Include ARIA labels for accessibility: `aria-label`, `aria-live`, `role`
- Keep inline event handlers (`onclick`) only for simple game logic triggers
- Structure: DOCTYPE → meta tags → style block → body → script block

## CSS

- **Organization:** Group related styles (board, buttons, animations, modal)
- **Naming:** Use descriptive class names (`.cell`, `.chip-btn`, `.result-banner`)
- **Responsive:** Mobile-first approach; breakpoints at 480px (mobile), 768px (tablet), 1200px (desktop)
- **Colors:** Use CSS variables or consistent hex values (e.g., `#ffd700` gold, `#8b0000` red)
- **Animations:** Prefer CSS `@keyframes` over JS animations (better performance)

## JavaScript

- **Variables:** Use `const` for constants, `let` for game state; no `var`
- **Naming:** camelCase for functions/variables (`selectCell`, `updateBalance`, `playNote`)
- **Scope:** Keep game state at the top level (balance, bets, rolling, activeChip, audioCtx, musicOn)
- **Functions:** Group by concern (betting, rolling, music, utilities)
- **Comments:** Use `// ──` section dividers and minimal inline comments (code should be self-documenting)
- **No minification:** Keep readable for browser DevTools debugging

## Single-File Structure

All code stays in `index.html`:
- `<style>` block (CSS only, no external stylesheets)
- `<body>` with semantic structure
- `<script>` block (all JavaScript)

Do not create separate files unless absolutely necessary (e.g., external images, audio files — currently none).
