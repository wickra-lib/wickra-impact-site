# Java

The Java binding links the C ABI via a small JNI shim. Construct an `Impact` from a
JSON spec and drive it with `command(json) -> json`.

```xml
<!-- Maven Central -->
<dependency>
  <groupId>org.wickra</groupId>
  <artifactId>wickra-impact</artifactId>
  <version>0.1.5</version>
</dependency>
```

```java
import org.wickra.impact.Impact;

// spec = ImpactSpec JSON (strategy + book_model + participation_cap + latency_ms)
try (Impact impact = new Impact(spec)) {
    String report = impact.command("{\"cmd\":\"run\",\"data\":{ /* symbol -> candles + book */ }}");
    System.out.println(report);
}
```

## More

- [central.sonatype.com/artifact/org.wickra/wickra-impact](https://central.sonatype.com/artifact/org.wickra/wickra-impact)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/java)
