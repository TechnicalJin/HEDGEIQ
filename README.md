# HedgeIQ Pro

HedgeIQ Pro is a single-file cricket hedge decision app.

It helps you monitor live odds, evaluate hedge timing, detect arbitrage, and lock a hedge with projected outcome profit for both match results.

## What This App Does

- Tracks live odds updates for both teams with custom labels (for example, overs or match moments).
- Runs a dual-mode engine:
  - Single Mode: timing-based hedging from odds drop and trend quality.
  - Arb Mode: activated when true arbitrage is detected from combined implied probabilities.
- Computes hedge amounts, total invested amount, and guaranteed minimum profit.
- Shows detailed analysis across tabs for signal, score, risk, timeline, history, and chart.
- Supports lock-and-freeze execution for both normal hedge and arb hedge flows.

## Core Model

The app evaluates each update in two layers.

### Layer 1: Mode Detection

Arbitrage index:

`arbIndex = (1 / oddsTeamA) + (1 / oddsTeamB)`

- If `arbIndex < 1.0` -> `ARB` mode.
- Else -> `SINGLE` mode.

Mode is recalculated on every odds update.

### Layer 2: Hedge Zone Classification (Single Mode)

Based on total drop in selected bet-team odds from entry:

- `WAIT`: drop `< 10%`
- `EARLY_HEDGE`: drop `10% to <20%`
- `STRONG_HEDGE`: drop `20% to <30%`
- `EXTREME`: drop `>= 30%`
- `REVERSAL`: current bet-team odds rise vs previous update while drop is `>= 20%`

## Signal Engine

Priority order:

1. `DAMAGE` when guaranteed result is deeply negative.
2. `ARB` in arb mode.
3. `REVERSAL`
4. `EXTREME`
5. `STRONG_HEDGE`
6. `EARLY_HEDGE`
7. `SAFE` when volatility is high.
8. `WAIT`

Signal labels used in UI include:

- `EARLY HEDGE`
- `STRONG HEDGE`
- `REVERSAL`
- `EXTREME - ACT NOW`
- `SAFE EXIT`
- `DAMAGE CONTROL`
- `ARB OPPORTUNITY`
- `HEDGE LOCKED` / `ARB LOCKED`

## Scoring System (0-100)

Weighted score used in analysis:

- Drop score: up to `40`
- Trend confirmations: up to `30`
- Stability score: up to `20`
- Confidence (update count): up to `10`

Total score drives hedge-quality guidance in the Score tab.

## Hedge and Profit Formulas

Given:

- Initial bet amount: `B`
- Entry odds (original bet side): `O1`
- Current odds (other side): `O2`

Hedge amount:

`H = (B * O1) / O2`

Projected profit if original bet side wins:

`profitBet = (B * O1) - B - H`

Projected profit if hedge side wins:

`profitHedge = (H * O2) - H - B`

Guaranteed estimate:

`guaranteed = min(profitBet, profitHedge)`

Arb mode uses the same hedge sizing structure, but only activates when `arbIndex < 1.0` and presents comparative output against normal hedge projection.

## Key UI Sections

- Match Setup: teams and initial odds.
- Bet Setup: selected side and stake.
- Live Signal Bar: current actionable signal, reversal alert, arb alert.
- Mode Banner and Zone Banner: current mode and single-mode zone guidance.
- Profit Output Card:
  - Hedge amount
  - Total invested
  - Guaranteed result
  - Single-mode decision engine (hedge now vs wait)
  - Arb comparison block (arb vs normal)

Tabs:

- Analysis: drop, volatility, risk meter, arb index meter, decision-condition checklist.
- Arb: live arb index, mode state, arb calculation panel, arb index history.
- Score: ring score and weighted breakdown bars.
- Chart: both team odds, early/strong zone lines, arb index line on secondary axis.
- Hedge: live hedge calculator, partial hedge options, apply/confirm panel.
- History: updates table and timeline of mode/zone/signal progression.

## Lock Workflow

When you confirm a hedge:

- A lock summary card appears (`HEDGE LOCKED` or `ARB LOCKED`).
- Inputs for further live updates are disabled.
- Locked odds and output are frozen for that session.
- Reset is required to start a new tracking cycle.

## Partial Hedge Support

In Single Mode, partial options appear in strong hedge conditions:

- `25%`
- `50%`
- `75%`
- `100%`

Selecting an option pre-fills the apply panel with projected outcome values.

## Project Structure

- `cricket_hedge_betting_system.html`: complete app (HTML, CSS, JavaScript in one file).
- `README.md`: project documentation.
- `SKILL.md`: workspace frontend design guidance.

## Run Locally

No build step is required.

1. Open `cricket_hedge_betting_system.html` in a modern browser.
2. Enter teams, opening odds, your selected bet side, and stake amount.
3. Click `START TRACKING`.
4. Add labeled live updates as the match progresses.
5. Watch mode/signal/zone changes and review hedge projections.
6. Use `APPLY HEDGE` or `EXECUTE ARB` when appropriate.
7. Confirm to lock values, or reset to begin a new session.

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- Chart.js (CDN)

## Limitations and Disclaimer

- This tool is a decision-support simulator, not financial advice.
- Market execution slippage, liquidity, delay, and exchange fees are not modeled.
- Always verify real exchange odds and stake constraints before placing actual bets.
