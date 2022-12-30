## Connecting log data to traces

You can manually enrich your Dynatrace ingested log data by defining a log pattern to include the `dt.span_id`, `dt.trace_id`, `dt.trace_sampled`, and `dt.entity.process_group_instance fields`.

Be sure to follow rules for the format of the enriched fields in an unstructured log. All these can be found in the reference documentation:

Reference: Dynatrace documentation [Connecting log data to traces](https://www.dynatrace.com/support/help/shortlink/log-monitoring-log-enrichment#opentelemetry-java)

### Before we start
The shopizer application has been pre-configured to use SLF4 logging framework [more inforomation here]().

For those interested in the Java configuration, you can refer to `application.properties` file in `sm-shop/src/main/resources` directory.

In the meantime, we will need to Configure oneagent to ingest logs from `/tmp/spring.log` as the log source.

Here are the steps

### What is required to connect the log data to traces?

>:warning: We will edit `TaggingStompSession.java` in the directory `sm-shop/src/main/java/com/salesmanager/shop/store/controller/product`

> **Warning** We will edit `TaggingStompSession.java` in the directory `sm-shop/src/main/java/com/salesmanager/shop/store/controller/product`

1. Import the `SpanContext` Java libs
1. Initialize the span context
1. Retrieve span and trace IDs

#### 1. Import the `SpanContext` Java libs

This has already been done for you in line 20

```java
import io.opentelemetry.api.trace.SpanContext;
```

#### 2. Initialize the span context

> **Your task:** Copy and paste the following code ***after*** the `try (Scope scope = span.makeCurrent())` statement

```java
SpanContext spanContext = Span.current().getSpanContext();
```

#### 3. Retrieve span and trace IDs

> **Your task:** Copy and paste the following code ***before*** the `return` statement, to write the followin data into the `spring.log` log file

```java
LOG.info("[!dt dt.trace_id=" + spanContext.getTraceId() + ",dt.span_id=" + spanContext.getSpanId() + "] sending message to " + headers.getDestination());
```

<details>
  <summary>Full code here</summary>
  
  ### Heading
  1. Foo
  2. Bar
     * Baz
     * Qux

  ### Some Code
  ```java
  try (Scope scope = span.makeCurrent()) {
	SpanContext spanContext = Span.current().getSpanContext();
	GlobalOpenTelemetry.getPropagators().getTextMapPropagator().inject(Context.current(), headers, this);
	LOG.info("[!dt dt.trace_id=" + spanContext.getTraceId() + ",dt.span_id=" + spanContext.getSpanId() + "] sending message to " + headers.getDestination());
	return session.send(headers, payload);
  } finally {
	span.end();
  }
  ```
</details>

Results

Browse log viewer.

Browse trace.