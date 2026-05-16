# Step D7 — Experiment lock (Filter #3: RSI depth)

**Status:** Code + `.set` ready — compile F7 → D7 IS/OOS vs D6 control  
**Date locked:** 2026-05-16

---

## Prerequisites

- Filters #1–#2 **kept** — `vem5m_d6_session_bbwidth.set` (session + BB width)
- Step E complete (E6 deferred)

---

## References

| Item | Path / value |
|------|----------------|
| Control | `MQL5/Profiles/Tester/vem5m_d6_session_bbwidth.set` |
| Test | `MQL5/Profiles/Tester/vem5m_d7_session_bb_rsi.set` |
| B5 evidence | [`step-b-complete-results.md`](step-b-complete-results.md) |
| D6 OOS control | 373 tr · −$4.58 · PF 0.96 · DD 8.4% |
| D6 IS | 724 tr · +$4.98 · PF 1.03 · DD 8.0% |

---

## Filter #3 — single hypothesis

**Name:** Deeper RSI at signal bar

**Hypothesis:** Entries on **shallow** band touches (RSI barely past 30/70) are net negative; require **deeper** oversold for longs and **deeper** overbought for shorts.

**Mechanism (one rule, two sides — same “depth” idea):**

| Side | Condition at signal bar (`inp_signal_shift`) |
|------|-----------------------------------------------|
| Long | `RSI <= inp_rsi_long_max_depth` (default **25**) — blocks shallow **25–30** bucket |
| Short | `RSI >= inp_rsi_short_min_depth` (default **75**) — blocks weak **70–75** zone |

Entry signal still uses `inp_rsi_os=30` / `inp_rsi_ob=70`; this filter **tightens** after raw signal passes.

**B5 (818-trade sample):**

| Bucket | Net P/L | Note |
|--------|---------|------|
| 25–30 (long) | −$10.05 | blocked by long max 25 |
| 70–75 (short) | −$1.77 | blocked by short min 75 |
| 75–80 (short) | −$12.95 | still allowed — fallback v7b if D7 fails |
| deep_<20 / 20–25 | positive | longs kept |

**Not in v1:** Change global `inp_rsi_ob`/`inp_rsi_os`, block 75–80, ADX/regime.

**Fallback v7b:** If D7 fails OOS, test `inp_rsi_short_min_depth=80` only (block 75–80).

---

## Evaluation windows

| Window | From | To |
|--------|------|-----|
| **IS** | 2024.01.01 | 2026.05.15 |
| **OOS** | 2025.01.01 | 2026.05.15 |

Compare vs **`vem5m_d6_session_bbwidth.set`** on **identical dates**.

---

## Pass / fail (D7)

**Keep filter #3** if vs D6 control:

- [ ] Net profit improves (IS and/or OOS)
- [ ] PF ≥ D6 (1.03 IS / 0.96 OOS) or clearly better risk-adjusted
- [ ] Max DD not materially worse
- [ ] Trade count remains sufficient (OOS rough floor **> 250**)

**Discard** if trades collapse with no PF gain.

---

## Deliverables

- [x] D7 D0 — this file  
- [x] `VEM_Risk_CheckRSIDepth` + inputs in `VEM_Config.mqh`  
- [x] `vem5m_d7_session_bb_rsi.set`  
- [ ] MetaEditor **F7** compile  
- [ ] D7 IS + OOS backtests → update `baseline-eurusd-m5-20260516.md`  
- [ ] D7 keep/discard  

---

## Expected shape

Moderate trade reduction (~10–25% vs D6) from removing shallow longs/shorts; net should improve if B5 holds on full tester sample.
