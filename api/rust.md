# Rust

The native crate. Run an impact-aware backtest with `run`, or drive an `Impact`
handle with the JSON command protocol every other binding uses.

```bash
cargo add wickra-impact
```

```rust
use impact_core::{run, ImpactSpec, RunData};

let spec = ImpactSpec::from_json(SPEC).expect("valid spec");
let data = RunData::from_json(DATA).expect("valid data");

let report = run(&spec, &data).expect("run");
println!("{:?}", report.stats);
```

## More

- [crates.io/crates/wickra-impact](https://crates.io/crates/wickra-impact) - [docs.rs](https://docs.rs/wickra-impact)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/rust)
- [ImpactSpec & book model](https://github.com/wickra-lib/wickra-impact/blob/main/docs/SPEC.md)
