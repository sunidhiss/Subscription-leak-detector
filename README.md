# subscription-leak-detector

A simple pandas program.
Detects forgotten recurring payments in transaction data using rule-based pattern matching.

## What it does

- Identifies monthly subscriptions from raw transaction history
- Flags payments above a configurable threshold as "leaks"
- Scores each subscription by cancellation difficulty
- Separates one-off transactions from recurring ones

## How it works

A transaction is classified as a subscription if it meets three conditions:
- Appears at least 3 times
- Payment gaps fall within a 25–35 day window
- Amount variance stays under ₹50 (tolerates minor price hikes)

## Tech

- Python, pandas
- Rule-based detection — no ML, no black box
- Configurable thresholds via constants at the top of the file

## Usage

```bash
pip install pandas
python subscription_detector.py
```

Replace the `data` dict with a CSV export from your bank or UPI app.

## Config

| Constant | Default | Description |
|---|---|---|
| `LEAK_THRESHOLD` | 300 | Monthly spend (₹) to flag as a leak |
| `MIN_OCCURRENCES` | 3 | Minimum payments to qualify as a subscription |
| `MONTHLY_WINDOW` | (25, 35) | Acceptable day range between payments |
| `AMOUNT_TOLERANCE` | 50 | Max standard deviation in payment amount (₹) |

## Sample output

```
Detected 3 subscription(s):

  • Gym          ₹   999/mo  (₹ 11988/yr)  Cancel: hard
  • Netflix      ₹   499/mo  (₹  5988/yr)  Cancel: medium
  • Spotify      ₹   119/mo  (₹  1428/yr)  Cancel: easy

Leaks (above ₹300/month):

  Gym — ₹999/month. Do you... actually go?
     Requires in-person visit or certified letter.

  Netflix — ₹499/month. Still watching?
     App cancellation works but buried under 3 menus.

──────────────────────────────────────────────────
  Monthly subscription total : ₹1,617
  Yearly  subscription total : ₹19,404
  Monthly leak total         : ₹1,498
  Estimated yearly waste     : ₹17,976
──────────────────────────────────────────────────
```
