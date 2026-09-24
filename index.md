---
layout: home
title: Wickra Impact — slippage measured, not guessed
titleTemplate: false

hero:
  name: "Wickra Impact"
  text: "The backtester that knows you moved the market."
  tagline: "Agent-based fills on the real historical L2 order book — your order is walked through recorded liquidity level by level, so slippage is measured, not guessed. Deterministic and byte-identical across ten languages."
  image:
    src: /wickra-mark.svg
    alt: Wickra Impact
  actions:
    - theme: brand
      text: View on GitHub
      link: https://github.com/wickra-lib/wickra-impact
    - theme: alt
      text: ImpactSpec & book model
      link: https://github.com/wickra-lib/wickra-impact/blob/main/docs/IMPACT_MODELS.md
    - theme: alt
      text: API
      link: /api/rust

features:
  - icon: 📉
    title: Fills that eat liquidity
    details: Every ordinary backtest fills your order at the close or a fixed slippage estimate, as if your size were invisible. Wickra Impact walks your order through the actual recorded L2 order book, level by level, so the fill price is what the market would really have given you.
  - icon: ♻️
    title: The backtest engine, 1:1
    details: "Impact inherits the wickra-backtest engine verbatim — its StrategySpec, RunRequest and BacktestReport — and replaces only the fill stage with an order-book-walk fill engine. Everything else is identical."
  - icon: 🎛️
    title: Model the friction
    details: "A book model, a participation cap and a latency knob shape how aggressively your order consumes the book — so you can stress a strategy against realistic execution, not a frictionless fantasy."
  - icon: 📈
    title: The same 514 indicators
    details: "A strategy's signals draw on any of the 514 indicators of the Wickra core, exactly as a plain backtest would — Impact changes only how the orders fill, not what they see."
  - icon: 🌐
    title: Ten languages, one report
    details: "The core is a JSON-over-C-ABI data API (Impact::command_json) in Rust, Python, Node.js, WASM, C, C++, C#, Go, Java and R. A developer in any language gets the same impact-aware report."
  - icon: 🧪
    title: Deterministic, proven
    details: A given request produces the byte-identical report in every binding, pinned by a golden corpus replayed through each binding in CI — the exact cross-language golden invariant.
---

<script setup>
const installTabs = [
  { label: 'Python', lang: 'bash', code: 'pip install wickra-impact' },
  { label: 'Node',   lang: 'bash', code: 'npm install wickra-impact' },
  { label: 'Rust',   lang: 'bash', code: 'cargo add wickra-impact' },
  { label: 'WASM',   lang: 'bash', code: 'npm install wickra-impact-wasm' },
  { label: 'C',      lang: 'bash', code: '# prebuilt header + library from GitHub releases:\n# github.com/wickra-lib/wickra-impact/releases' },
  { label: 'C#',     lang: 'bash', code: 'dotnet add package Wickra.Impact' },
  { label: 'Go',     lang: 'bash', code: 'go get github.com/wickra-lib/wickra-impact-go' },
  { label: 'Java',   lang: 'xml',  code: '<!-- Maven Central -->\n<dependency>\n  <groupId>org.wickra</groupId>\n  <artifactId>wickra-impact</artifactId>\n  <version>0.1.5</version>\n</dependency>' },
  { label: 'R',      lang: 'r',    code: 'install.packages("wickraimpact", repos = "https://wickra-lib.r-universe.dev")' },
]

const pyCode = `import json
from wickra_impact import Impact

spec = json.dumps({
    "strategy": {
        "spec_version": 1, "symbol": "IMPACT", "timeframe": "1h", "indicators": {},
        "entry": {"ge": [{"price": "close"}, 0]}, "exit": {"in_position": True},
        "sizing": {"type": "fixed_qty", "qty": 10.0},
        "execution": {"order_type": "market", "fill_timing": "next_open"},
    },
    "book_model": {"kind": "orderbook_walk"}, "participation_cap": 1.0, "latency_ms": 0,
})

impact = Impact(spec)
data = {"IMPACT": [{"time": 0, "open": 100, "high": 101, "low": 99, "close": 100, "volume": 1000}]}
report = json.loads(impact.command(json.dumps({"cmd": "run", "data": data})))
print(report["stats"])  # carries the market impact a naive backtest hides`

const nodeCode = `import { Impact } from 'wickra-impact'

const spec = JSON.stringify({
  strategy: {
    spec_version: 1, symbol: 'IMPACT', timeframe: '1h', indicators: {},
    entry: { ge: [{ price: 'close' }, 0] }, exit: { in_position: true },
    sizing: { type: 'fixed_qty', qty: 10.0 },
    execution: { order_type: 'market', fill_timing: 'next_open' },
  },
  book_model: { kind: 'orderbook_walk' }, participation_cap: 1.0, latency_ms: 0,
})

const impact = new Impact(spec)
const data = { IMPACT: [{ time: 0, open: 100, high: 101, low: 99, close: 100, volume: 1000 }] }
const report = JSON.parse(impact.command(JSON.stringify({ cmd: 'run', data })))
console.log(report.stats)`

const cliCode = `# Run an impact-aware backtest over recorded L2 book data:
wickra-impact --spec impact.json --data ./data

# Raw ImpactReport JSON — the same bytes every binding returns:
wickra-impact --spec impact.json --data ./data --format json`

const snippetTabs = [
  { label: 'Python', lang: 'python',     code: pyCode },
  { label: 'Node',   lang: 'javascript', code: nodeCode },
  { label: 'CLI',    lang: 'bash',       code: cliCode },
]
</script>

## The spec is JSON, not code

An `ImpactSpec` is a `strategy` (the same `StrategySpec` a backtest runs), a
`book_model`, a `participation_cap` and a `latency_ms`. The strategy decides what
to trade; the book model decides how the fill eats the recorded liquidity.

```json
{
  "strategy": {
    "spec_version": 1, "symbol": "BTCUSDT", "timeframe": "1h",
    "indicators": { "ema": { "type": "Ema", "params": [20] } },
    "entry": { "cross_above": ["close", "ema"] },
    "exit":  { "cross_below": ["close", "ema"] },
    "sizing": { "type": "fixed_qty", "qty": 5.0 },
    "execution": { "order_type": "market", "fill_timing": "next_open" }
  },
  "book_model": { "kind": "orderbook_walk" },
  "participation_cap": 0.2,
  "latency_ms": 50
}
```

## Install

The same impact-aware backtest from every language — native Rust, Python, Node.js
and WASM, plus a C ABI for C, C++, C#, Go, Java and R.

<InstallTabs :tabs="installTabs" />

## Run it from any language

Construct an `Impact` from the JSON spec, then drive it with
`command(json) -> json`. Every binding returns the same impact-aware report.

<InstallTabs :tabs="snippetTabs" />

## Built on the Wickra core

Wickra Impact is part of the [Wickra](https://wickra.org) ecosystem. It inherits
the [`wickra-backtest`](https://github.com/wickra-lib/wickra-backtest) engine over
the 514 indicators of [`wickra-core`](https://github.com/wickra-lib/wickra), and
changes only how orders fill — so a strategy sees exactly the same numbers, but
pays the slippage it would really have paid.

> Wickra Impact is a software library, not a trading system, and comes with no
> warranty — a backtest is not financial advice. Use at your own risk.
