# About Wickra Impact

Wickra Impact is the backtester that knows you would have moved the market. It
walks your order through the real historical L2 order book, eating liquidity level
by level, so slippage is **measured, not guessed**. A run is a JSON document —
**data, not code** — so it executes in every one of ten languages and returns a
byte-for-byte identical report.

## What makes it different

- **Fills that eat liquidity.** Ordinary backtests fill at the close or a fixed
  slippage estimate, as if your size were invisible. Impact walks your order through
  the actual recorded L2 book, level by level, so the fill price is what the market
  would really have given you.
- **The backtest engine, 1:1.** Impact inherits the `wickra-backtest` engine
  verbatim — its `StrategySpec`, `RunRequest` and `BacktestReport` — and replaces
  **only** the fill stage with an order-book-walk fill engine.
- **Model the friction.** A book model, a participation cap and a latency knob shape
  how aggressively your order consumes the book, so you can stress a strategy against
  realistic execution.
- **The same indicators.** A strategy's signals draw on the 514 indicators of the
  Wickra core, exactly as a plain backtest would — Impact changes only how the orders
  fill, not what they see.

## Why it exists

Backtests flatter strategies by pretending fills are free. Wickra Impact makes the
fill honest — modelled **once**, in Rust, over the real recorded book — and exposes
it as a JSON-over-C-ABI data API to Rust, Python, Node.js, WASM and — over a C ABI —
C, C++, C#, Go, Java and R. The same impact-aware run executes identically anywhere.

## Open source

Released under the **MIT OR Apache-2.0** license — permissive, OSI-approved, free
for any use including commercial. Source, issues and releases on
[GitHub](https://github.com/wickra-lib/wickra-impact).

## Disclaimer

Wickra Impact is a software library, **not** a trading system, and is provided
**as-is with no warranty**. An impact-aware backtest is a simulation over historical
data, not financial advice, and does not guarantee future execution. Use it at your
own risk.
