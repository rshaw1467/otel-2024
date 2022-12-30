## Span capturing in Dynatrace

### How to instrument Java applications with Open Telemetery?

Reference: Dynatrace documentation [Manually instrument Java applications with OpenTelemetry](https://www.dynatrace.com/support/help/extend-dynatrace/opentelemetry/opentelemetry-traces/opentelemetry-ingest/opent-java)

1. Add dependencies to your project
1. Import the claases
1. Acquiring a Tracer
1. Create spans
1. Configure context propagation

Open

```
sm-shop/src/main/java/com/salesmanager/shop/store/controller/category/ShoppingCategoryController.java
```

Line 767 to 769 sets declares the tracer

```java
	public static Tracer getTracer() {
		return GlobalOpenTelemetry.getTracer("shop-category", "1.0.0");
	}
```

Look at Line 144 to 154

Line 145, to create Spans you only need to specify the name of the span. Here, we name the span `query-categories`

```java
Span span = getTracer().spanBuilder("query-categories").startSpan();
```

This matches what you see in the "distrubted traces" in Dynatrace.

Line 146 makes the span the current span
```java
try (Scope scope = span.makeCurrent()) {
    // In this scope, the span is the current/active span
    // your applicaiton code/logic goes here.
}
```
Line 152 is mandatory as it is  required to call end() to end the span when you want it to end.
```java
finally {
    span.end();
}
```

Pieceing it together, you will normally wrap your code with the followwing

```java
Span span = getTracer().spanBuilder("query-categories").startSpan();
try (Scope scope = span.makeCurrent()) {
  // Your code
} finally {
    span.end();
}
```

### See the results
Go to Dynatrace menu -> distributed traces, seach for the the transaction `category`, you should see the following

Click on any one of the traces. The application we are working with is indeed already augmented with OpenTelemetry. The developer has chosen to signal to monitoring solutions which portions of the application code  are of importance.

### Control span capturing without writing code

In this specific case the developer might have been a bit too overzealous. The Span `query-category` is visible countless times within the PurePath.

You might only need to track the span of `query-categories` from an operations perspective, rather than all the interations of each `query-category`.

You can use the Dynatrace UI to configure what spans you need without having to write code.

### Task

> **Your Task:** Use Dynatrace UI to ignore the span named `query-category`.

Navigate to `Settings > Server-side service monitoring > Span capturing`

![name](../../../assets/images/04-04-xx.png)

In our case it's sufficient enough to specify the span name that should be excluded. But you'll notice that the configuration allows for much more specific exclusion rules.

Result:

![name](../../../assets/images/04-04-xx.png)