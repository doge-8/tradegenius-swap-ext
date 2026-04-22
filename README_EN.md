# TG Auto Swap - Chrome Extension

**Language**: [中文](README.md) | **English**

An auto-swap tool for TradeGenius that loops A↔B swaps between any two saved tokens.

This is a Chrome extension — please run it on the official Chrome browser; no security concerns.

**Current version**: v4.0.0

## ✨ Features

- **Auto A↔B looping**: cycles swaps between two saved tokens, no manual clicks
- **Fixed amount per swap**: set a constant amount each round to protect your principal from full-balance swaps
- **Auto-return to start token on stop**: stop at any time; the extension automatically swaps the leftover B back to your designated A
- **Trade-settings protection**: save your ideal slippage / aggregator / priority config as a template; the extension auto-restores it before every run
- **Daily scheduled start**: set a time and it auto-runs every day
- **Multi-chain token support**: pick which chain to trade for tokens like USDT / USDC that exist on multiple chains
- **Gas monitoring**: real-time gas price for 10+ chains so you can avoid peak hours
- **Debug mode**: dry-run the whole flow without actually executing swaps

## 📦 Install

1. Download the latest `tg-swap-ext.zip` from [Releases](https://github.com/doge-8/tradegenius-swap-ext/releases)
2. Unzip it
3. Open Chrome and go to `chrome://extensions/`
4. Toggle **Developer mode** (top-right)
5. Click **Load unpacked**
6. Select the unzipped folder

## 🔑 License Activation

A license key is required on first use:

1. Open the extension — the activation page will appear
2. Enter the license key, format: `XXXX-XXXX-XXXX-XXXX`
3. Click **Activate**

Join the [Telegram group](https://t.me/+ILxE5DrnpHA2OWRl) to contact the author for a **3-day free trial key**.

For a paid license key, DM on Twitter or reach out in the TG group.

## 🚀 Usage

### 1. Configure tokens

1. Open `https://www.tradegenius.com/zh/trade` (**language must be set to Chinese**)
2. On the site, add the two tokens you want to swap to **Saved**:
   - The first (top) = **A start**: the token you want to keep / return to
   - The second (bottom) = **B target**: the token that gets consumed
3. Click the extension icon → **Settings** tab → **Fetch tokens**
4. Verify the token cards: A/B badge colors are correct (green=A, gray=B). Click **Set as A** to swap if needed.

### 2. Trade settings

| Parameter | Description | Default |
|---|---|---|
| **Amount per swap** | Amount of A used per A→B round (0 = MAX / full balance) | 0 |
| **Max rounds** | Upper limit on number of swaps (0 = unlimited) | 10 |
| **Multi-chain target** | For multi-chain tokens, **manually pick** the chain from the dropdown (required — can't start without it) | — |

### 3. Risk protection (recommended)

| Parameter | Description | Default |
|---|---|---|
| **Slippage guard** | If the preview slippage exceeds this value → refresh and retry. Genius charges 0.05% fee; **0.055% is recommended** (slightly above fee to cover on-chain friction) | 0 (off) |
| **Gas guard** | Auto-monitors gas on the chain chosen in **Trade settings**; pauses when gas exceeds this threshold and auto-resumes when it drops | 0 (off) |

### 4. Execution parameters

| Parameter | Description | Default |
|---|---|---|
| Interval between swaps | Wait time between rounds | 5 s |
| Gas refresh interval | How often gas price is polled | 10 s |
| **Min swap value** | Don't initiate a swap if the source token balance is below this (USD) | 10 |
| **Min leftover value** | On stop, if B is below this, treat it as "returned to A" (USD) | 0.5 |
| **Debug mode** | Stops before clicking the final confirm button — no real swap | Off |

### 5. Trade-settings protection (recommended)

To prevent site settings from being accidentally changed mid-run:

1. First, in the TradeGenius trade-settings panel (gear icon on the right side of P3), adjust your ideal config: slippage, aggregator, chain, priority gwei, etc.
2. Go back to the extension → **Settings** → find the **Trade-settings protection** card → click **Capture current settings as template**
3. Tick the **Enable protection** checkbox (can't be ticked before a template is captured)
4. Before every loop start, the extension will auto-calibrate the site config back to your saved template

### 6. Scheduled start (optional)

- Switch to the **Schedule** tab
- Tick **Enable scheduled start** and pick a time (24h format, e.g. `10:00`)
- The loop will auto-start at this time every day
- **The browser must stay open** for it to fire
- If a run is already in progress when the scheduled time hits, the trigger is skipped

### 7. Run

1. Switch to the **Swap** tab
2. Make sure the status card shows **Ready** (connected)
3. Click **Start**
4. If trade-settings protection is enabled, you'll see **Calibrating site settings...** first, then it enters **Running**

**Common pre-flight blocks** (shown as a popup when you click Start):
- Tokens not fetched
- Multi-chain token detected but target chain not selected
- Trade-settings protection enabled but no template captured

## 📊 Trade Statistics

- **Pink numbers**: current-round volume (resets on extension reload)
- **Yellow numbers**: cumulative lifetime volume (persisted)
- **×  button**: clear history

> Statistics are for reference only — trust actual on-chain records.

## 🎨 Status Reference

| Status | Meaning |
|---|---|
| **Ready** | Connected to TradeGenius, can start |
| **Running** | Auto-swap loop in progress |
| **Returning to A...** | Post-stop catch-up flow |
| **Stopped (back to A)** | Catch-up done; holding A |
| **Stopped (B leftover $X)** | B still has a residual balance; handle manually |
| **Stopped (catch-up failed)** | On-chain failure (usually high slippage) |
| **Not connected** | Open the TradeGenius trade page and switch to Chinese |
| **License invalid / expired** | Re-activate the license key |

## 🔒 Multi-chain Safety

All multi-chain swaps go through strict validation:

- **Source panel**: matches the chain version by chain-badge icon
- **Target panel**: matches by normalized hover-menu text
- **Target chain not found → stops immediately with an error** — never picks the wrong chain

## ⚠️ Notes

- Keep the TradeGenius page in the foreground while running
- First-time users: enable **Debug mode** to dry-run the flow (no gas consumed)
- Set reasonable gas / slippage thresholds
- A should ideally be a stablecoin (USDT / USDC), combined with a fixed **Amount per swap** to lock the round size

## ❓ FAQ

**Q: The extension shows "Not connected" — what now?**
A: Refresh the TradeGenius page, make sure you're on `/trade`, and switch the language to Chinese.

**Q: License activation failed?**
A: Double-check the key, or click **Verify** to refresh the status.

**Q: Swaps keep failing?**
A:
- Check wallet balance and gas
- Increase the slippage threshold
- Check the slippage value in your trade-settings protection template
- Try a manual swap on the site to confirm it can succeed

**Q: Catch-up fails?**
A: The catch-up flow has 3 built-in retries. If it still fails (usually slippage guard), manually clear the B leftover on the site.

**Q: Scheduled start didn't fire?**
A: The browser must stay open and the TradeGenius page must be connected. Triggers that occur while the browser is closed are NOT caught up later.

## 📥 Download

Latest version: [Releases](https://github.com/doge-8/tradegenius-swap-ext/releases/latest)

---

## 💬 Contact

- **Author**: 岳来岳会赚
- **X (Twitter)**: [@188888_x](https://x.com/188888_x)
- **Telegram**: [专注币圈黑科技](https://t.me/+ILxE5DrnpHA2OWRl)

## ❤️ Support the Author

If this tool helps you, please consider registering with the referral code below — it really helps!

**Referral link**: [tradegenius.com/ref/MO2GAW](https://www.tradegenius.com/ref/MO2GAW)
