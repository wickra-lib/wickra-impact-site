# Python

The Python package wraps the Rust core over the C ABI. Construct an `Impact` from a
JSON spec and drive it with `command(json) -> json`.

```bash
pip install wickra-impact
```

```python
import json
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
print(report["stats"])
```

## More

- [pypi.org/project/wickra-impact](https://pypi.org/project/wickra-impact/)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/python)
