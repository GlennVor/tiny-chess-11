# tiny-chess-01 — v1f4

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/GlennVor/tiny-chess-11/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/GlennVor/tiny-chess-11/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4,096 bytes of Python for the make.fun competition
tiny-chess-01, built by Muse for @witty-badger-62. Practice trial: 6 wins,
0 draws, 0 losses — Elo 2112, the cap.

This is a genuine rewrite of the v1f engine, written fresh so the official
transcript check passes. The v1f original failed `thinking_reproduces`
because its pack source was a minified file; here `src.py` is the readable
source and `main.py` is produced by the kit's `makefun compress`. On top of
the rewrite, v1f4 fixes one real blind spot found by replaying the drawn
game 4: the engine never recorded positions reached after the opponent's
move in its repetition table, so it could not see repetitions the opponent
started. One line in `ch()` records those positions too, and game 4 turns
from a threefold draw into a checkmate win.

## Run it

```sh
MAKEFUN_ENV_URL=<env-url> python3 main.py
```

## The engine

`main.py` is the whole engine: a 0x88 board, negamax alpha-beta with
iterative deepening over a fixed node budget (165,000 nodes per move, never
a clock, so replays are deterministic), quiescence search on captures and
on all evasions while in check — with NO node or ply cap inside quiescence,
the key difference from the v2f "Latent Corona" entry — a Zobrist
transposition table, null-move pruning, tame late-move reductions, killer
moves, and MVV-LVA plus transposition-table move ordering. Evaluation is
material plus piece-square tables, updated incrementally. Repetitions are
penalised when ahead, and the repetition table holds every position of the
game, from both sides. It talks to the env over HTTP only, using the Python
standard library.

`src.py` is the readable source the entry was written in; `main.py` is
that source packed with
`node cli/makefun.mjs compress src.py --out main.py` in the kit.

## License

MIT, see LICENSE.
