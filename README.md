# Tiny Chess Engine (sub-4 KB)

[![Competition](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![Rank](https://make.fun/badge/tiny-chess-01/alvenze/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![Reward](https://make.fun/badge/tiny-chess-01/alvenze/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A complete, deterministic chess engine in a single `main.py` of 4,045 bytes
(bounty cap: 4,096), written for make.fun's "Tiny chess engine" bounty.

## How it works

- Full legal move generation: castling with transit checks, en passant,
  promotions, check evasions.
- Negamax alpha-beta search with a transposition table, principal variation
  search, quiescence folded into the search, and null-move pruning.
- Move ordering: TT move first, then MVV-LVA captures.
- Evaluation: material plus piece-square tables.
- Fixed 60,000-node budget with a fixed-seed Zobrist key: no clock, no
  randomness, so replays reproduce move for move.
- The source is stored as an lzma-compressed payload that `main.py` unpacks
  and execs (the bounty starter explicitly allows this form); the payload
  and the unpacker together are the 4,045 bytes.

## Run it

The engine talks to the bounty environment at `MAKEFUN_ENV_URL`
(`POST /game`, `/move`, `/resign`) and plays its games autonomously:

    MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 -P -s main.py

Python 3.13, standard library only. `main.py` reads, imports, and runs no
other file in this repository.

Local practice (kit env + scorer, three trials): Elo 1451 on every trial
against the house gauntlet (4 wins, 2 losses each time), above the 1,154
finalist floor and above the 4 KB sunfish baseline (mean 1365).
