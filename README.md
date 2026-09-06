# Kryptonite Profile

A single-page, static backtest for **BTC, ETH, XRP, SOL, PAXG, XAUT, PANW** (Palo Alto Networks stock) **and the S&P 500** (via the SPY ETF). You log the
purchases you actually made - coin, what you spent, and the date - and the app looks
up that day's closing price, then tracks what each entry is worth now and over time.

## What it does

- **Buy entries.** Each entry = coin + amount + purchase date, typed in the currency
  selected in the corner (AED / CAD / USDT). The app fetches the daily close for that
  exact date and values the holding as `amount x price(now) / price(buy date)`.
- **Totals in every currency.** What you own, what you put in, and profit/loss are
  shown in AED, CAD and USDT side by side. "Put in" converts your spend at the buy
  date's FX rate - the money you actually parted with, not a moving target.
- **Charts.** Price history for the four coins (% change or log price) and the
  portfolio value curve, which steps up as entries come in.
- **Encrypted vault.** Everything you save is AES-256-GCM encrypted in your browser's
  localStorage with a key derived from your passkey (PBKDF2-HMAC-SHA256, 600k
  iterations, random salt). The passkey is never stored or transmitted; a wrong
  passkey simply fails to decrypt. There is no recovery if you forget it.

## Privacy model

The page is static - there is no backend and no account. Your entries never leave
your browser, and the repository contains no secrets of any kind: authentication is
the ability to decrypt your own local data. Crypto prices come from Binance (CryptoCompare fallback); PANW from stockanalysis.com
(split/dividend-adjusted daily closes, native CORS, no key); FX history from Frankfurter
(ECB reference rates). AED uses the fixed 3.6725 peg and USDT is counted 1:1 with USD.

## Running it

Open `index.html`, or serve the folder statically:

```bash
python -m http.server 8899
```

Hosted at: https://offport.github.io/kryptonite-profile/

**Cross-device:** your encrypted vault lives only in the browser it was made in (per-origin
localStorage, never on GitHub). To use it on another device, press **Backup** to download the
encrypted file, move it over, and choose **Restore from a backup file** on the lock screen — the
same passkey then unlocks the same data. The backup is ciphertext; it is useless without the passkey.
