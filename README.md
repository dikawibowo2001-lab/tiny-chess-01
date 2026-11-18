# Tiny Chess — Build the strongest chess engine in 4 KB of Python

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/dikawibowo2001-lab/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/dikawibowo2001-lab/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A make.fun bounty entry (`tiny-chess-01`): a chess engine in at most
4,096 bytes of Python. `main.py` is the whole engine — it plays the
house gauntlet through the env at `MAKEFUN_ENV_URL`, searching to a
fixed node budget (never a clock), so every trial replays move for
move.

## The engine

- 0x88 mailbox board, pseudo-legal move generation, make/unmake with
  incremental material + piece-square evaluation and Zobrist hashing.
- Negamax alpha-beta with iterative deepening to a fixed node budget
  (150,000 nodes per move), transposition table, null-move pruning,
  quiescence over captures, promotions and replies to check, and
  MVV-LVA capture ordering.
- Piece-square tables for pawn, knight, bishop and king (queens and
  rooks are material-only); the king uses a middlegame table while
  non-pawn material is high and switches to an endgame table below
  3000 centipawns of non-pawn material. Bishop-pair bonus included.
- The source is packed as an lzma payload with its unpacker; payload
  and unpacker together stay under the 4,096-byte cap.

## Practice results (manual harness — `grade-local` cannot run Python
## bots on this machine, so trials were run against the kit's own env
## server and scorer)

| trial | W–D–L | Elo |
|-------|-------|-----|
| 0 | 3–1–2 | 1364 |
| 1 | 5–0–1 | 1654 |
| 2 | 3–0–3 | 1279 |
| **mean** | | **1432** |

House baseline (sunfish 4k) averages ~1365 on these trials. Per-move
time over 878 trial moves: average 1.8s, slowest 3.7s.

## Grade it locally

    node tiny-chess-01-kit/cli/makefun.mjs grade-local --kit tiny-chess-01-kit --bot . --driver process --instances 3
