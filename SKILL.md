---
name: billionaire-crash-html
description: "Standalone HTML casino crash game 'Billionaire Crash' for Stake Engine. Use this skill for ANY work on the billionaire-crash-html repo: gameplay changes, UI polish, math tuning, adding mechanics, sound design, animations, bug fixes. Trigger on mentions of 'crash game', 'billionaire crash html', 'казино html игра', 'stake game html', or any request to modify the standalone crash game prototype."
---

# Billionaire Crash — Standalone HTML Edition

Single-file HTML5 casino crash game. Canvas rendering, Web Audio sounds, full math engine, Stake-style dark UI. Zero dependencies — opens in any browser.

## НАСТРОЙКА ОКРУЖЕНИЯ

```bash
cd /home/claude/billionaire-crash-html
git pull origin main 2>/dev/null || true
```

Для просмотра: `python3 -m http.server 3001` или просто открыть `index.html`.

## АРХИТЕКТУРА (single file: index.html)

```
index.html
├── CSS (~300 lines)
│   ├── Layout: CSS Grid (canvas + sidebar)
│   ├── Fonts: Orbitron (display) + JetBrains Mono (numbers) + Manrope (body)
│   ├── Theme: Stake-style dark (#0a0e17 bg, #00e701 green, #ff3b5c red, #ffd700 gold)
│   └── Components: top-bar, canvas-area, sidebar, bottom-bar
│
├── HTML (~120 lines)
│   ├── Top bar (logo + balance)
│   ├── Canvas area (game canvas + multiplier overlay + history + event banner + win popup)
│   └── Sidebar (mode tabs, bet input, action button, auto-cashout, auto-bet, stats)
│
└── JavaScript (~600 lines)
    ├── CONFIG — all constants (house edge, growth rate, modes, event weights)
    ├── MathEngine — crash point generation, event selection, round generation
    ├── SoundEngine — Web Audio synthesis (bet, launch, tick, cashout, crash, etc.)
    ├── GameRenderer — Canvas 2D rendering (stars, rocket, trail, particles, obstacles, collectibles)
    ├── State — reactive game state object
    ├── DOM — cached element references
    ├── UI functions — updateBalance, updateMultiplier, showEventBanner, etc.
    ├── Game logic — startRound, launchFlight, gameTick, checkEvents, cashOut, crash
    └── Render loop — requestAnimationFrame
```

## МАТЕМАТИЧЕСКАЯ МОДЕЛЬ

### RTP = 96%

Crash point: `max(1.00, floor(0.96 / random * 100) / 100)`
- ~4% instant crash (1.00×) = house edge
- Exponential distribution гарантирует E[1/crash] = 0.04

### Growth: `e^(0.06 * tick * 0.05)` = e^(0.003 * tick)

Multiplier растёт экспоненциально. Tick = 50ms.
`multiplierToTick(m) = ln(m) / 0.003`
`tickToMultiplier(t) = e^(0.003 * t)`

### 5 механик (event weights):

| Mechanic       | Weight | RTP Share | Trigger Range | Effect              |
|---------------|--------|-----------|---------------|---------------------|
| base_crash    | 52.04% | 52%       | —             | Чистый crash        |
| mini_boss     | 11.74% | 11.7%     | 2.0×–8.0×     | +1.2×–2.5× bonus   |
| debris        | 8.00%  | 8%        | 1.5×–4.0×     | −20%–50% penalty   |
| boss_rage     | 7.93%  | 7.9%      | 3.0×–12.0×    | +1.5×–5.0× bonus   |
| golden_rocket | 16.00% | 16%       | 1.5×–6.0×     | +2.0×–10.0× bonus  |

### 4 bet modes:

| Mode           | Cost Mult | Win Cap   |
|---------------|-----------|-----------|
| base          | 1×        | 5,000     |
| business_class| 10×       | 50,000    |
| vip_executive | 25×       | 125,000   |
| ceo_mode      | 100×      | 500,000   |

## ТЕКУЩЕЕ СОСТОЯНИЕ

### Готово ✅
- [x] Math engine с 96% RTP
- [x] Canvas рендеринг (ракета, звёзды, trail, частицы, obstacles, collectibles)
- [x] Stake-style UI (тёмная тема, Orbitron font, responsive grid)
- [x] 5 игровых механик с visual feedback
- [x] 4 bet modes с разными win caps
- [x] Web Audio synthesis (9 звуков)
- [x] Auto-cashout по заданному множителю
- [x] Auto-bet (автоматический следующий раунд)
- [x] История последних 15 раундов (badges)
- [x] Event banners (mini-boss, debris, boss rage, golden rocket)
- [x] Win popup с анимацией
- [x] Screen shake при событиях
- [x] Particle system (engine trail, explosions)
- [x] Keyboard support (Space = bet/cashout)
- [x] Session P/L tracking
- [x] Countdown (3...2...1) перед запуском

### TODO 🔧
- [ ] Sprite-based rocket (вместо Canvas drawing)
- [ ] Real sound effects (вместо Web Audio synthesis)
- [ ] Turbo mode (быстрая анимация)
- [ ] Multiplier graph (кривая как в Stake crash)
- [ ] Провайдерская полоска "Provably Fair" с seed verification
- [ ] Mobile touch optimization
- [ ] Bet presets (quick bet buttons: $1, $5, $10, $50)
- [ ] Round history panel (подробная таблица)
- [ ] Звёздное поле parallax (несколько слоёв)
- [ ] Debris visual dodge animation
- [ ] Boss fight mini-animation
- [ ] Win streak / loss streak tracking
- [ ] Sound on/off toggle в UI

## КРИТИЧЕСКИЕ ПРАВИЛА

1. **Один файл** — вся игра в `index.html`. Никаких внешних зависимостей кроме Google Fonts.
2. **Math constants** — все в объекте `CONFIG`. Не хардкодить числа в логике.
3. **RTP** — 96.0000%. Формула `0.96 / Math.random()`. Не менять без пересчёта.
4. **Event amounts** — бонусы в единицах base bet, НЕ scaled.
5. **Win cap** — применяется ПОСЛЕ всех бонусов: `Math.min(winAmount, modeCfg.winCap)`.
6. **Canvas rendering** — requestAnimationFrame loop, отделён от game tick (setInterval 50ms).
7. **Git workflow**: `git add -A && git commit -m "description" && git push origin main`

## СВЯЗЬ С ОРИГИНАЛОМ (billionaire-crash)

Это **та же игра по концепции**, но другая реализация:

| Аспект | Оригинал (billionaire-crash) | HTML Edition (этот репо) |
|--------|------------------------------|--------------------------|
| Stack | Svelte 5 + PixiJS 8 + XState 5 + npm | Vanilla HTML/CSS/JS, zero deps |
| Рендеринг | PixiJS (WebGL) | Canvas 2D API |
| Стейт | XState 5 actor | Простой объект State |
| Звуки | WAV файлы | Web Audio synthesis |
| Ассеты | 65 PNG спрайтов + видео | Canvas-drawn графика |
| RGS | stake-engine npm (real API) | Локальный MathEngine (симуляция) |
| Сборка | SvelteKit + Vite + adapter-static | Нет сборки, один файл |
| Деплой | Stake Engine ACP | Любой статик-хостинг |

Математика ИДЕНТИЧНА: те же веса, формулы, RTP 96%, 4 modes, 5 механик.
Визуал УПРОЩЁН: canvas-drawn ракета вместо спрайтов, но layout и UX те же.

## GIT

- GitHub: https://github.com/2211bogdan2012-blip/billionaire-crash-html
- Branch: main
