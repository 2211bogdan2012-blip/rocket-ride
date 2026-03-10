# 🚀 Billionaire Crash — HTML Edition

**Standalone crash casino game in a single HTML file.** Zero dependencies, opens in any browser.

<p align="center">
  <strong>Stake-style dark UI • 96% RTP • 5 game mechanics • Canvas 2D rendering</strong>
</p>

## Play

```bash
# Option 1: Just open it
open index.html

# Option 2: Local server
python3 -m http.server 3001
# → http://localhost:3001
```

## Game Mechanics

🎰 **Base Crash** (52%) — Classic crash: multiplier grows, cash out before it crashes  
⚔️ **Mini-Boss** (11.7%) — Enemy spawns, defeat it for 1.2×–2.5× bonus  
☄️ **Debris** (8%) — Asteroid field with −20%–50% penalty  
🔥 **Boss Rage** (7.9%) — Epic boss fight, 1.5×–5.0× bonus  
🚀 **Golden Rocket** (16%) — Transform into gold, 2.0×–10.0× bonus  

## Bet Modes

| Mode | Cost | Win Cap |
|------|------|---------|
| Base | 1× | $5,000 |
| Business Class | 10× | $50,000 |
| VIP Executive | 25× | $125,000 |
| CEO Mode | 100× | $500,000 |

## Controls

- **Space** — Place bet / Cash out
- **Mouse** — All UI interactions
- Auto-cashout at target multiplier
- Auto-bet for continuous play

## Tech

- **Rendering**: Canvas 2D (stars, rocket, trails, particles, obstacles)
- **Sound**: Web Audio API synthesis
- **Math**: Provably fair crash point generation (96% RTP)
- **UI**: CSS Grid, Google Fonts (Orbitron + JetBrains Mono)
- **Size**: Single HTML file, ~1200 lines

## Math

```
crashPoint = max(1.00, floor(0.96 / random() × 100) / 100)
multiplier(t) = e^(0.06 × t × 0.05)
```

RTP verified: `E[1/crash] = houseEdge = 0.04` → **96.0000% RTP**

---

Built for [Stake Engine](https://stakeengine.com) by NEXTUP MEDIA / Flame Generation LLC.
