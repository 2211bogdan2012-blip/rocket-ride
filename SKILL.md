---
name: rocket-ride
description: "Standalone HTML casino crash game 'Rocket Ride' for Stake Engine. Use this skill for ANY work on the rocket-ride repo: gameplay changes, UI polish, math tuning, adding mechanics, sound design, animations, bug fixes. Trigger on mentions of 'crash game', 'rocket ride', 'казино html игра', 'stake game html', or any request to modify the standalone crash game prototype."
---

# Rocket Ride — Standalone HTML Edition

Single-file HTML5 casino crash game. Canvas rendering, Web Audio sounds, full math engine, Stake-style dark UI. Zero dependencies — opens in any browser.

## НАСТРОЙКА ОКРУЖЕНИЯ

```bash
cd /home/claude
git clone https://2211bogdan2012-blip:ghp_TOKEN@github.com/2211bogdan2012-blip/rocket-ride.git
cd rocket-ride
```

Для просмотра: `python3 -m http.server 3001` или просто открыть `index.html`.

## АРХИТЕКТУРА (single file: index.html, ~1200 lines)

```
├── CSS (~300 lines) — Grid layout, Orbitron/JetBrains Mono/Manrope, Stake dark theme
├── HTML (~120 lines) — Top bar, canvas area, sidebar, bottom bar
└── JS (~600 lines)
    ├── CONFIG — constants (house edge, growth rate, modes, event weights)
    ├── MathEngine — crash point, event selection, round generation
    ├── SoundEngine — Web Audio synthesis (9 sounds)
    ├── GameRenderer — Canvas 2D (stars, rocket, trail, particles, obstacles, collectibles)
    ├── State — game state object
    ├── UI — updateBalance, updateMultiplier, showEventBanner, etc.
    ├── Game logic — startRound, gameTick, checkEvents, cashOut, crash
    └── Render loop — requestAnimationFrame
```

## МАТЕМАТИКА

**RTP = 96%.** Crash point: `max(1.00, floor(0.96 / random * 100) / 100)`
Growth: `e^(0.003 * tick)`, tick = 50ms.

### 5 механик:

| Mechanic       | Weight | Trigger Range | Effect              |
|---------------|--------|---------------|---------------------|
| base_crash    | 52.04% | —             | Чистый crash        |
| mini_boss     | 11.74% | 2.0×–8.0×     | +1.2×–2.5× bonus   |
| debris        | 8.00%  | 1.5×–4.0×     | −20%–50% penalty   |
| boss_rage     | 7.93%  | 3.0×–12.0×    | +1.5×–5.0× bonus   |
| golden_rocket | 16.00% | 1.5×–6.0×     | +2.0×–10.0× bonus  |

### 4 bet modes:

| Mode           | Cost | Win Cap   |
|---------------|------|-----------|
| base          | 1×   | 5,000     |
| business_class| 10×  | 50,000    |
| vip_executive | 25×  | 125,000   |
| ceo_mode      | 100× | 500,000   |

## STAKE ENGINE СОВМЕСТИМОСТЬ

✅ Stateless rounds, pre-computed outcomes, RTP 96%, original IP, no Stake branding
⚠️ Cash out визуальный (singleRoundWin) — уточнить с review team
❌ Для production нужно: Svelte 5 + PixiJS 8, math books, RGS API

## СТАТУС

**Готово:** Math 96% RTP, Canvas renderer, Stake UI, 5 механик, 4 modes, Web Audio, auto-cashout, auto-bet, history, events, particles, keyboard, session P/L.

**TODO:** Multiplier graph, better visuals, turbo mode, mobile touch, bet presets, sound toggle.

## ПРАВИЛА

1. Один файл `index.html`, zero deps
2. Все константы в `CONFIG`
3. RTP 96% — `0.96 / Math.random()`
4. Win cap после бонусов
5. Git: `git add -A && git commit -m "msg" && git push origin main`

## GIT

- GitHub: https://github.com/2211bogdan2012-blip/rocket-ride
- Branch: main
