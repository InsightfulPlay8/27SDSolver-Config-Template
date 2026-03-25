# 27SD Solver — Config Template

Copy `template.json`, fill in your game settings, and send it back. I'll run the solve and send you the strategy file.

## How to fill it in

Edit these fields to match your game:

### Game Setup

| Field | What it means | Examples |
|-------|--------------|----------|
| `effective_stack` | Starting stack in big blinds | `20`, `30`, `50`, `100` |
| `small_blind` | Small blind amount | `0.5` |
| `big_blind` | Big blind amount | `1.0` |
| `ante` | Dead money posted before the hand | `0` (no ante), `1` (1bb ante) |
| `max_draw` | Maximum cards a player can draw | `5` (standard), `3` (restricted) |

### Rules

| Field | What it means | Default |
|-------|--------------|---------|
| `heads_up` | Heads-up (2 players) | `true` |
| `no_limp` | No limping allowed (fold or raise only) | `false` |
| `no_open_jam` | Can't open-shove pre-draw (must use sized raise) | `false` |

### Bet Sizes

Sizes can be written as:
- **Pot fractions**: `0.33` = 33% pot, `0.75` = 75% pot, `1.0` = pot
- **BB amounts**: `"2.5bb"` = raise to 2.5 big blinds total
- **All-in**: `"AI"`

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

## Example: 20bb, 2.5x open, 3x 3-bet, pot-sized post-draw bets

```json
{
  "effective_stack": 20,
  "ante": 0,
  "no_limp": true,
  "no_open_jam": true,
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

## Example: 50bb deep, with ante, multiple raise sizes

```json
{
  "effective_stack": 50,
  "ante": 1,
  "no_limp": false,
  "max_draw": 5,
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

## Fields you can leave out

Any field not in your JSON uses the default. You only need to include what you want to change. The minimum config is:

```json
{
  "effective_stack": 20
}
```

This runs a standard 20bb HU game with default bet sizes.
