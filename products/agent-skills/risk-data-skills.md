---
description: Deterministic risk and market-data feeds from the gdex-skill package — HyperLiquid market risk and anomaly events, a token security screen, and GVault accounting — plus a bring-your-own-key harness
---

# 📊 Risk & Market Data Skills

{% hint style="info" %}
These four skills produce **data, not trades**. Each ships a standard-library Python script that
prints NDJSON (one JSON record per line). They need no API key, no install beyond Python 3, and no
Gemach infrastructure. Every number comes from the script's own reads, never from a model.
{% endhint %}

| Skill | What it emits | Source |
|-------|---------------|--------|
| `gdex-hl-market-risk` | One record per HyperLiquid core perp (~234): funding, open interest, oracle premium, mark/oracle/mid, leverage cap, delisted flag | GDEX HyperLiquid API |
| `gdex-hl-anomaly` | Scored, time-stamped anomaly **events** for HyperLiquid core perps, plus one coverage record per run | HyperLiquid public `info` API |
| `gdex-token-risk` | GDEX token screen across 12 chains: price, liquidity, volume, honeypot flag, buy/sell tax, LP lock, holder concentration | GDEX trending API |
| `gvault` | GVault (GMACL, Enzyme vault on Ethereum): NAV, share price, holdings, cumulative and annualised return | Enzyme's on-chain accounting |

## Install and run

```bash
npx skills add GemachDAO/gdex-skill --skill gdex-hl-market-risk
# or clone the repo and run any script directly:
python3 skills/gdex-hl-market-risk/scripts/hl_market_risk.py > hl.ndjson
python3 skills/gdex-token-risk/scripts/token_risk.py --chain-id 622112261 8453 > tokens.ndjson   # Solana, Base
python3 skills/gvault/scripts/gvault.py
python3 skills/gdex-hl-anomaly/scripts/hl_anomaly.py --hours 24
```

{% hint style="warning" %}
**Missing is not safe.** Token security fields are sparse. The honeypot flag, for example, is
present on only about a quarter of rows. A field that is absent is emitted as `null` and must never
be scored as "passed".
{% endhint %}

## HyperLiquid anomaly events

`gdex-hl-anomaly` answers "what just happened that is unusual *for this market*?" It flags three
categories:

| Category | Fires when |
|----------|-----------|
| `oracle_divergence` | The hourly mark-vs-oracle premium is far from the market's own baseline |
| `funding_extremity` | The 8-hour mean funding is far from baseline **and** the premium is outside HyperLiquid's funding clamp band. The record always carries the premium. |
| `liquidity_shock` | Bar volume or bar range spikes far above baseline (spikes only) |

**How "unusual" is measured.**

- Each market is compared with its own trailing 7 days, using the median and median absolute
  deviation. One past spike cannot hide the next one.
- Each category has its own threshold, calibrated on quiet control periods, never on the events
  it was later tested against.

**What each event carries.**

- `detected_at`: when the anomaly happened, not when the job ran.
- A robust `z` and the threshold it crossed.
- A 0–1 `score`, which ranks events but is not a probability.
- `model_version`.
- An `evidence_ref`: the public endpoint, the time, and a hash of the exact baseline, so any event
  can be recomputed.

{% hint style="info" %}
Every run ends with an `anomaly_coverage` record listing the window and which markets were and
were not scored. **No event means nothing crossed a threshold in a market that was scored, not
that the market was normal.**
{% endhint %}

**Backtest.** The skill was tested on three real events:

| Event | Result |
|-------|--------|
| JELLY squeeze, 26 Mar 2025 | Detected 1h after onset |
| 10 Oct 2025 liquidation cascade | 19 of 20 control markets flagged |
| POPCAT pump-and-dump, 12 Nov 2025 | Detected in the same hour |

- All three events were detected.
- Control markets produced **0.044 false alarms per market-day**.

The detector reports anomalies as they happen; it does not predict them. Full method and limits
are in the skill's `APPROACH.md` and `backtest/RESULTS.md`.

## Bring-your-own-key harness

The package's [`harness/`](https://github.com/GemachDAO/gdex-skill/tree/main/harness) runs these
skills with Claude on **your own** Anthropic API key, so model usage is billed to you and Gemach
hosts nothing.

```bash
pip install -r harness/requirements.txt
export ANTHROPIC_API_KEY=sk-ant-...        # yours
python harness/gemach_harness.py export --out feed.ndjson          # no model; every feed
python harness/gemach_harness.py ask "Which HyperLiquid markets trade furthest from oracle?"
```

The harness has two modes:

- **`export`** needs no key and runs no model. It fails closed: a non-zero exit if any feed fails
  or returns nothing, so a partial feed is never published.
- **`ask`** runs an agent that can only quote figures a skill script printed.

Every script run, in both modes, is logged with its arguments, its exit code and a sha256 of its
output.
