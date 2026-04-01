# HedgeIQ Pro

HedgeIQ Pro is a single-file web app for tracking live cricket odds and deciding when to hedge a pre-match bet.

It helps you:
- Track odds updates for both teams over time.
- Evaluate hedge timing using a signal engine (`HEDGE`, `EARLY`, `WAIT`, `EXIT`).
- Calculate hedge amount and projected profit in both match outcomes.
- Visualize odds movement and trend quality with charts and score breakdowns.

## Project Structure

- `cricket_hedge_betting_system.html` - Main application (UI, logic, and styling in one file).
- `SKILL.md` - Frontend design guidance used in this workspace.

## Features

- Match setup with team names and opening odds.
- Bet setup with side selection and stake amount.
- Live odds update feed with labeled checkpoints.
- Decision engine based on:
  - Odds drop percentage
  - Trend confirmations
  - Volatility/stability
  - Number of updates (confidence)
- Hedge timing score (`0-100`) with weighted factors.
- Risk meter and recommendation signals.
- Profit output panel showing:
  - Hedge amount
  - Total invested
  - Guaranteed profit estimate
- Missed best hedge-point warning when odds rebound.
- Odds history table and chart view.

## Hedge Formula Used

Given:
- Initial bet amount: `B`
- Entry odds (bet team): `O1`
- Current odds (opposite team): `O2`

Hedge amount:

`H = (B * O1) / O2`

Profit projections:

- If original bet team wins: `(B * O1) - B - H`
- If opposite team wins: `(H * O2) - H - B`

Guaranteed profit estimate:

`min(profit_if_bet_team_wins, profit_if_opposite_team_wins)`

## Run Locally

No build step is required.

1. Open `cricket_hedge_betting_system.html` in any modern browser.
2. Enter teams, initial odds, selected bet side, and stake.
3. Start tracking and add live odds updates as the match progresses.
4. Use the signal and hedge tabs to decide when to apply hedge.

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- [Chart.js](https://www.chartjs.org/) via CDN

## Notes

- This tool provides decision support and simulations, not financial advice.
- Real market behavior can change rapidly; always verify live exchange values before placing hedge bets.
