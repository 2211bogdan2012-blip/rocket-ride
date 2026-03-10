# Billionaire Crash — Game Design Document (HTML Edition)

## Концепция

Crash-hybrid iGaming игра с тематикой "бизнес-империя". Игрок запускает ракету, множитель растёт экспоненциально, нужно нажать CASH OUT до краша. По пути — 5 уникальных механик (мини-боссы, дебрис, золотая ракета и т.д.).

## Целевая аудитория

- Игроки Stake.com, Duelbits, Roobet (18+)
- Любители crash-игр (Aviator, Spaceman, Limbo)
- Высокие ставки: CEO Mode × 100

## Отличия от стандартного Crash

1. **5 механик** вместо чистого crash — каждый раунд уникален
2. **4 bet modes** — от casual (1×) до whale (100×)
3. **Visual events** — мини-боссы, дебрис, золотая трансформация
4. **Бизнес-тема** — "Launch the Boss", корпоративная сатира

## Flow раунда

```
[IDLE] → Player bets → [COUNTDOWN 3..2..1] → [FLYING]
                                                  ↓
                                        Multiplier grows (e^0.003t)
                                        Events trigger at thresholds
                                                  ↓
                              ┌─ CASH OUT pressed → [WIN] → payout
                              └─ Crash point hit → [CRASHED] → loss
```

## Математика

См. SKILL.md — полная таблица весов, RTP модель, формулы.

## Монетизация

House edge = 4%. На 100K раундов при средней ставке $10:
- Total wagered: $1,000,000
- Expected GGR: $40,000 (4%)
- RTP to players: $960,000 (96%)
