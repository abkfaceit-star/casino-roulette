---
name: casino-mcp
description: Play provably fair casino games (roulette, dice, coin flip, crash, blackjack, plinko) through the Purple Flea Casino MCP server. Use when the user asks to place a bet, spin the roulette, check casino balance, deposit/withdraw, verify a bet proof, size a bet with Kelly Criterion, or run tournaments/PvP challenges.
---

# Purple Flea Casino MCP

The project registers the `purple-flea-casino` MCP server in `.mcp.json`
(npm package `@purpleflea/casino-mcp`, source: https://github.com/purple-flea/casino-mcp).
Tools appear as `mcp__purple-flea-casino__<tool>`.

## Setup

1. Node.js 18+ must be installed; the server starts via `npx -y @purpleflea/casino-mcp`.
2. API key: either export `PURPLE_FLEA_API_KEY=sk_live_...` (see `.env.example`)
   or call the `register` tool once — the key is set automatically for the session.
   Save the returned key: it is shown only once.
3. Base URL defaults to `https://casino.purpleflea.com` (`PURPLE_FLEA_URL`).

## Workflow for a bet

1. `balance` — confirm funds before betting. All amounts are in USD.
2. `kelly_limits` — get the max recommended bet per game; never exceed it
   unless the user explicitly overrides.
3. Place the bet (see tools below). Pass a `client_seed` if the user wants
   to verify fairness later.
4. `verify_proof` with the returned bet id to check the HMAC-SHA256 proof.

Real money is involved. Always confirm the amount with the user before a bet,
withdrawal, tournament entry or challenge, and report the exact result the
server returns (win/loss, payout, new balance).

## Roulette (primary game for this project)

Tool `roulette`:

| Param | Values |
|-------|--------|
| `bet_type` | `number`, `red`, `black`, `odd`, `even`, `high`, `low`, `dozen_1..3`, `column_1..3` |
| `bet_value` | 0-36, required only for `number` |
| `amount` | USD, positive |
| `client_seed` | optional string |

Payouts: number 35.28x, color/odd-even/high-low 1.96x, dozens/columns 2.94x.
European wheel (single zero), 0.5% house edge.

## Other games

- `flip` — `side` heads/tails, `amount`. 1.96x.
- `dice` — roll 1-100, over/under a `threshold` (`direction`), `amount`.
- `crash` — `target_multiplier` 1.01-1000, `amount`.
- `custom_bet` — `win_probability` 1-99, `amount`.
- `blackjack` — `amount`, then `action` hit/stand/double.
- `plinko` — `rows` 8/12/16, `risk` low/medium/high, `amount`.
- `batch_bet` — up to 10 bets in one call; each item has `game`
  (`coin_flip`, `dice`, `multiplier`, `roulette`, `custom`, `blackjack`, `plinko`) plus that game's params.

## Account, bankroll, social

- Funds: `deposit_address` (chain: base, ethereum, arbitrum, optimism, polygon,
  solana, monero, bitcoin, lightning), `withdraw` (`amount`, `address`; $0.50 fee, min $1), `history`.
- Bankroll: `kelly_optimal` (`game` coin_flip/dice_over/dice_under/custom,
  optional `threshold`, `win_probability`, `risk_factor` 0.1-1.0), `kelly_limits`,
  `stats_me`, `stats_leaderboard` (optional `game`).
- Tournaments: `tournament_create`, `tournament_enter`, `tournament_play`,
  `tournament_leaderboard`. Prizes split 60/30/10.
- PvP: `challenge_create`, `challenge_accept`, `challenge_list` (2% house cut on escrow).
- Referrals: `gossip` (no auth), `referral_stats`, `referral_withdraw`.

## Troubleshooting

- Tools missing: run `/mcp` in Claude Code and approve the `purple-flea-casino`
  project server; make sure `npx` is on PATH.
- `401`/unauthorized: the key is unset or wrong — set `PURPLE_FLEA_API_KEY` or call `register`.
