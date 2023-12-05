## OpenTelemetry metric definition in Python

In this section we go through OpenTelemetry Metric setup and create a new metric. 

Reference: Dynatrace documentation
- [OpenTelemetry metric concepts](https://www.dynatrace.com/support/help/shortlink/opentelemetry-metric-concepts)
- [OpenTelemetry instrument code samples](https://www.dynatrace.com/support/help/shortlink/opentelemetry-instrument-examples)

1. MeterProvider (MetricReader/Exporter) configuration
1. Instrument definition
1. Recording a measurement (sync & async)
1. Finding your metric in Dynatrace

---

### 1. Meter & Instrument Definition
#### 👂 Listen & follow in code

Navigate to the following file:

```
src/main/resources/jvm.envionrment.properties
```

OpenTelemetry AutoInstrumentor metrics configuration (sending to otel collector so no authenticaiton to Dynatrace is needed):

```java
FRONTEND.OTEL_METRICS_EXPORTER=otlp
FRONTEND.OTEL_EXPORTER_OTLP_METRICS_PROTOCOL=http/protobuf
FRONTEND.OTEL_EXPORTER_OTLP_METRICS_ENDPOINT=http://localhost:4318/v1/metrics
# FRONTEND.OTEL_EXPORTER_OTLP_METRICS_HEADERS=Authorization=Api-Token <token>
FRONTEND.OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE=DELTA
```

Navigate to the following file:

```
src/main/shop/FrontendServer.java
```

We start by importing our otel libraries:

```java
import io.opentelemetry.api.GlobalOpenTelemetry;
import io.opentelemetry.api.OpenTelemetry;
import io.opentelemetry.api.common.AttributeKey;
import io.opentelemetry.api.common.Attributes;
import io.opentelemetry.api.metrics.LongCounter;
import io.opentelemetry.api.metrics.Meter;
```

Instantiate our opentelemetry configuration in the object `openTelemetry`:

```java
private static final OpenTelemetry openTelemetry = GlobalOpenTelemetry.get();
```

Use `openTelemetry` to create our Meter object `meter`:

```java
private static final Meter meter = openTelemetry.meterBuilder("manual-instrumentation").setInstrumentationVersion("1.0.0").build();
```

Use `meter` to create our Instruments:

```java
private static final LongCounter confirmedPurchasesCounter = meter.counterBuilder("shop.purchases.confirmed").setDescription("Number of confirmed purchases").build();
```
```java
private static final LongCounter expectedRevenueCounter = meter.counterBuilder("shop.revenue.expected").setDescription("Expected revenue in dollar").build();
```
```java
private static final LongCounter actualRevenueCounter = meter.counterBuilder("shop.revenue.actual").setDescription("Actual revenue in dollar").build();
```

Comment out ActualRevenue for the people to uncomment.

We can use Expected revenue to count the attempted purchases as well. Bonus, create an ExpectedPurchases counter to count this with an otel metric. 

### 📌 Task: Create an additional instrument

**Your Task:** Create an addtional LongCounter to track attempted Purchases

**1.1** In the file `src/main/shop/FrontendServer.java` on line 37 create a new LongCounter object called `expectedPurchaseCounter` (similar to line 34,35,36) with the following properties:
- name:`"shop.purhcases.expected"`
- description:`"Number of expected purchases"`

<details>
  <summary>Expand to copy and paste the code</summary>

  ```java
private static final LongCounter attemptedPurchases = meter.counterBuilder("shop.purhcases.attempted").setDescription("Number of attempted purchases").build();
  ```
</details>

---

### 2. Passing Measurments 
#### 👂 Listen & follow in code

We can see how metrics are populated on line 75:

```java
private static void reportPurchases(Product product) {
    Attributes attributes = Attributes.of(AttributeKey.stringKey("product"), product.getName());
    confirmedPurchasesCounter.add(1, attributes);
}
```
```java
private static void reportExpectedRevenue(Product product) {
    Attributes attributes = Attributes.of(AttributeKey.stringKey("product"), product.getName());
    expectedRevenueCounter.add(product.getPrice(), attributes);
}	
```
```java
private static void reportActualRevenue(Product product) {
    Attributes attributes = Attributes.of(AttributeKey.stringKey("product"), product.getName());
    actualRevenueCounter.add(product.getPrice(), attributes);
}
```

As our code is running, these functions are called to pass our measurments - like in line 53, 69, and 70:

```diff
public static String handlePlaceOrder(HttpExchange exchange) throws Exception {
		// log.info("Frontend received request: " + exchange.getRequestURI().toString());
		Product product = Product.random();
		String productID = product.getID();
+		reportExpectedRevenue(product);
		try (Connection con = Database.getConnection(10, TimeUnit.SECONDS)) {...
```
```diff
public static String handlePurchaseConfirmed(HttpExchange exchange) throws Exception {
	String requestURI = exchange.getRequestURI().toString();
	String productID = requestURI.substring(requestURI.lastIndexOf("/") + 1);
	Product product = Product.getByID(productID);

+	reportPurchases(product);
+	reportActualRevenue(product);

	return "confirmed";
}
```
### 📌 Task: Pass a measurment 

**Your Task:** Call the newly created function in the code to pass a measurment

**2.1** In the file `src/main/shop/FrontendServer.java` after line 89 create a new function called `reportExpectedPurchases`, taking `product` as a parameter, and adding 1 to our expectedPurchases `instrument`.

Hint: this will be almost exactly the same as the `reportPurchases` function starting on line 76. 

<details>
  <summary>Expand to copy and paste the code</summary>

  ```java
	private static void reportExpectedPurchases(Product product) {
        Attributes attributes = Attributes.of(AttributeKey.stringKey("product"), product.getName());
        expectedPurchasesCounter.add(1, attributes);
	}
  ```
</details>

**2.2** Between line 54 and 55 call the `reportExpectedPurchases` function passing `product` as an argument. 

Hint: this will be almost exactly the same as the call to `reportExpectedRevenue` on line 54. 

<details>
  <summary>Expand to copy and paste the code</summary>

  ```java
		reportExpectedPurchases(product);
  ```
</details>

Then restart your applicaiton:

<gif of restarting application >

**2.3** As part of the OpenTelemetrey auto-isntrumentation some metrics are provided out of the box depending on the libraries and technologies being used.

Naviate to the Metric Explorer and search for `process.runtime` to see the metrics that are provided out of the box. 

Next search `shop` to see the custom metrics that were pre-populated and the newly created metric opentelemetry metric.

Depending on the libraries being use there may be some metrics provided out of the box. 

---

### Summary

To summarize what we've done:
- We intialized OpenTelemetry which created our MeterProvider as part of the GlobalOpentelmetry object
- We created a Meter instance and used that to create our Insturments
- We passed a metric to our instrument as part of our business logic
- We could see that out of the box, depending on libraries used, metrics will also be created with automatic instrumentation 

```java
+------------------+
| MeterProvider    |                 +-----------------+             +--------------+
|   Meter A        | Measurements... |                 | Metrics...  |              |
|     Instrument X +-----------------> In-memory state +-------------> MetricReader |
|     Instrument Y |                 |                 |             |              |
|   Meter B        |                 +-----------------+             +--------------+
|     Instrument Z |
|     ...          |                 +-----------------+             +--------------+
|     ...          | Measurements... |                 | Metrics...  |              |
|     ...          +-----------------> In-memory state +-------------> MetricReader |
|     ...          |                 |                 |             |              |
|     ...          |                 +-----------------+             +--------------+
+------------------+
```

