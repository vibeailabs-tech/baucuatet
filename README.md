# Bầu Cua Tôm Cá 🎊

A Vietnamese Tết dice gambling game playable in any browser. No dependencies, no install — just one HTML file.

![Game Preview](https://raw.githubusercontent.com/vibeailabs-tech/baucuatet/main/preview.png)
> *Screenshot: red festive board with 6 symbol cells, 3 dice, and gold betting controls.*

## Play instantly

Download or clone the repo, then open `index.html` in your browser:

```bash
git clone https://github.com/vibeailabs-tech/baucuatet.git
cd baucuatet
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

## How to play

1. Click one or more symbol cells to select them
2. Click a chip button (+10, +50, +100, +500) — it adds that amount to **all** selected cells
3. Use **2×** to double your current bets, or **Tất cả** to go all-in on selected cells
4. Click **Lắc!** to roll the 3 dice
5. Win if your symbol appears on 1, 2, or 3 dice — pays 1×, 2×, or 3× your bet
6. Click the **🔇 Nhạc** button (top-right) to toggle background music

You start with **1,000 xu**. A game-over dialog appears when you run out.

## Symbols

| Symbol | Vietnamese | English |
|--------|-----------|---------|
| 🎃 | Bầu | Gourd |
| 🦀 | Cua | Crab |
| 🦐 | Tôm | Shrimp |
| 🐟 | Cá | Fish |
| 🐓 | Gà | Rooster |
| 🦌 | Nai | Deer |

## Payout table

| Matches | Payout |
|---------|--------|
| 1 die   | 1× bet |
| 2 dice  | 2× bet |
| 3 dice  | 3× bet |
| 0 dice  | lose bet |

## Features

- Festive Tết red-and-gold design
- 3-dice animated roll
- Bet on multiple symbols at once
- Double-down and all-in buttons
- Background music (Web Audio API, no files needed)
- Round history (last 10 rounds)
- Mobile-friendly tap targets
- Keyboard accessible
