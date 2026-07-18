# R

The R package links the C ABI. Build an impact backtest with `wkimpact_new`, then
drive it with `wkimpact_command`.

```r
install.packages("wickraimpact", repos = "https://wickra-lib.r-universe.dev")
```

```r
library(wickraimpact)

# spec = ImpactSpec JSON (strategy + book_model + participation_cap + latency_ms)
impact <- wkimpact_new(spec)
data <- '{"IMPACT":[{"time":0,"open":100,"high":101,"low":99,"close":100,"volume":1000}]}'
report <- wkimpact_command(impact, paste0('{"cmd":"run","data":', data, '}'))
cat(report)
```

## More

- [wickra-lib.r-universe.dev](https://wickra-lib.r-universe.dev)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/r)
