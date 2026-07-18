# C\#

The .NET binding wraps the C ABI. Construct an `Impact` from a JSON spec and drive
it with `Command(json) -> json`.

```bash
dotnet add package Wickra.Impact
```

```csharp
using Wickra.Impact;

// spec = ImpactSpec JSON (strategy + book_model + participation_cap + latency_ms)
using var impact = new Impact(spec);
var report = impact.Command("{\"cmd\":\"run\",\"data\":{ /* symbol -> candles + book */ }}");
Console.WriteLine(report);
```

## More

- [nuget.org/packages/Wickra.Impact](https://www.nuget.org/packages/Wickra.Impact)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/csharp)
