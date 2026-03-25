# 27SD Solver — Config Template

## How to fill it in

Edit these fields to match your game:

### Game Setup

| Field | What it means | Examples |
|-------|--------------|----------|
| `effective_stack` | Starting stack in big blinds | `20`, `30`, `50`, `100` |
| `small_blind` | Small blind amount | `0.5` |
| `big_blind` | Big blind amount | `1.0` |
| `ante` | Dead money posted before the hand (single amount, not per-player) | `0` (no ante), `1` (1bb ante) |
| `max_draw` | Maximum cards a player can draw | `5` (standard), `3` (restricted) |

### Rules

| Field | What it means | Default |
|-------|--------------|---------|
| `heads_up` | Heads-up (2 players) | `true` |
| `no_limp` | No limping allowed (fold or raise only) | `false` |
| `auto_ai_spr` | All-in is only auto-added when stack/pot ratio is below this. Set to `0` for always available. Explicit `"AI"` in a size list always works regardless. | `0` |

### Bet Sizes

Sizes can be written as:
- **Pot fractions**: `0.33` = 33% pot, `0.75` = 75% pot, `1.0` = pot
- **BB amounts**: `"2.5bb"` = raise to 2.5 big blinds total
- **All-in**: `"AI"` (always available even if `auto_ai_spr` would suppress it)

| Field | What it means |
|-------|--------------|
| `ip_pre_raise` | IP (button/SB) opening raise size |
| `oop_pre_raise` | OOP (BB) 3-bet size |
| `oop_pre_4bet` | OOP 4-bet size |
| `ip_pre_4bet` | IP 4-bet size |
| `oop_post_bet` | OOP post-draw bet sizes |
| `oop_post_raise` | OOP post-draw raise sizes |
| `ip_post_bet` | IP post-draw bet sizes |
| `ip_post_raise` | IP post-draw raise sizes |

Multiple sizes are allowed — just add them to the array:
```json
"oop_post_bet": [0.33, 0.67, 1.0, "AI"]
```

## Example: 20bb, 2.5x open, 3x 3-bet, no open jam

All-in only when SPR < 3 (auto), but always available as a 4-bet (explicit `"AI"`):

```json
{
  "effective_stack": 20,
  "ante": 0,
  "no_limp": true,
  "auto_ai_spr": 3.0,
  "bet_sizes": {
    "ip_pre_raise": ["2.5bb"],
    "oop_pre_raise": ["7.5bb"],
    "oop_pre_4bet": ["AI"],
    "ip_pre_4bet": ["AI"],
    "oop_post_bet": [0.33, 0.67, 1.0],
    "oop_post_raise": ["AI"],
    "ip_post_bet": [0.33, 0.67, 1.0],
    "ip_post_raise": ["AI"]
  }
}
```

## Example: 50bb deep, with ante, multiple sizes

```json
{
  "effective_stack": 50,
  "ante": 1,
  "no_limp": false,
  "auto_ai_spr": 4.0,
  "bet_sizes": {
    "ip_pre_raise": ["2.5bb"],
    "oop_pre_raise": ["8bb"],
    "oop_pre_4bet": ["22bb", "AI"],
    "ip_pre_4bet": ["22bb", "AI"],
    "oop_post_bet": [0.25, 0.5, 1.0, "AI"],
    "oop_post_raise": [0.75, "AI"],
    "ip_post_bet": [0.25, 0.5, 1.0, "AI"],
    "ip_post_raise": [0.75, "AI"]
  }
}
```

## Example: 30bb, all-in always available

Set `auto_ai_spr` to `0` (or leave it out):

```json
{
  "effective_stack": 30,
  "auto_ai_spr": 0,
  "bet_sizes": {
    "ip_pre_raise": ["2bb"],
    "oop_pre_raise": ["6bb"],
    "oop_pre_4bet": ["AI"],
    "ip_pre_4bet": ["AI"],
    "oop_post_bet": [0.5, 1.0, "AI"],
    "oop_post_raise": ["AI"],
    "ip_post_bet": [0.5, 1.0, "AI"],
    "ip_post_raise": ["AI"]
  }
}
```

## Fields you can leave out

Any field not in your JSON uses the default. You only need to include what you want to change. The minimum config is:

```json
{
  "effective_stack": 20
}
```

This runs a standard 20bb HU game with default bet sizes and all-in always available.
