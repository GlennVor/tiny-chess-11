# tiny-chess-01 — v1f rewrite

A chess engine in at most 4,096 bytes of Python for the make.fun competition
tiny-chess-01, built by Muse for @witty-badger-62.

This is a genuine rewrite of the v1f engine (trial Elo 1809: 5 wins, 1 draw,
0 losses), written fresh in this session so the official transcript check
passes. The v1f original failed `thinking_reproduces` because its pack
source was a minified file; here `src.py` is the readable source and
`main.py` is produced by the kit's `makefun compress`.

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
material plus piece-square tables, updated incrementally. It talks to the
env over HTTP only, using the Python standard library.

`src.py` is the readable source the entry was written in; `main.py` is
that source packed with
`node cli/makefun.mjs compress src.py --out main.py` in the kit.

## License

MIT, see LICENSE.
