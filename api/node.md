# Node.js

The Node package is a native napi addon over the Rust core. Construct an `Impact`
from a JSON spec and drive it with `command(json) -> json`.

```bash
npm install wickra-impact
```

```javascript
import { Impact } from 'wickra-impact'

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
console.log(report.stats)
```

## More

- [npmjs.com/package/wickra-impact](https://www.npmjs.com/package/wickra-impact)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/node)
