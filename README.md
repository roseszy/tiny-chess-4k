# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/roseszy/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/roseszy/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A tiny chess engine in 4,096 bytes of Python, written for the [make.fun
bounty "Build the strongest chess engine in 4 KB of Python"](https://make.fun/bounty/tiny-chess-01) (tiny-chess-01).

## What it is

An original 0x88 alpha-beta engine: iterative deepening to a fixed node
budget, quiescence search over captures/promotions/check evasions,
transposition table, MVV-LVA capture ordering and killer moves, and an
evaluation of material plus piece-square tables and the bishop pair.
Deterministic: the same position always gets the same move (fixed node
count, no clock, no unseeded randomness).

`main.py` holds the engine as a compressed payload (lzma + base85) with a
tiny unpacker, as the bounty allows; the payload and unpacker together
are 3,033 bytes, under the 4,096-byte limit. The readable source it
unpacks is the same engine, developed and tested locally: move generation
is verified against standard perft suites (startpos, Kiwipete and four
more positions, depths 1-4), and it searches a fixed 150,000 nodes per
move (about 1.4-2.9 s/move), deterministically.

## How it plays

It talks to the house env at `$MAKEFUN_ENV_URL`: POST `/game {}` to get
the game in play, POST `/move {"game": n, "move": "<uci>"}` to play, and
repeats until the env answers `{"status": "done"}`.

    MAKEFUN_ENV_URL=http://localhost:PORT python3 main.py

Python 3.13, standard library only.
