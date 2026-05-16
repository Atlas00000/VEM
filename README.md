# VEM — Volatility Expansion Mean Reversion (MT5)

EURUSD M5 mean-reversion EA: Bollinger + RSI + volume spike entries; optional habitat filters (session, BB width).

## Install (MetaTrader 5)

Copy into your terminal `MQL5` folder (merge folders):

- `Experts/VEM/` → `MQL5/Experts/VEM/`
- `Include/VEM/` → `MQL5/Include/VEM/`
- `Profiles/Tester/vem5m*.set` → `MQL5/Profiles/Tester/`

Compile `Experts/VEM/VEM.mq5` in MetaEditor (F7).

## Working tester profile

`Profiles/Tester/vem5m_d6_session_bbwidth.set` — session block 13–15 + wide BB filter.

Baseline (filters off): `vem5m.set`.

## Docs

Edge discovery playbook: `Experts/VEM/edge-discovery.md`  
Baseline metrics: `Experts/VEM/baseline-eurusd-m5-20260516.md`
