## Trace context propagtion between 2 Java services

You might notice in the distributed traces that there is a "standalone" span called <mark>**RECEIVE /app/chat/java**</mark> that is not connected to any traces.

![websocket span](../../../assets/images/04-04-websocketserver-span1.png)

Let's take a closer look at the meta-data of the span.

![websocket span](../../../assets/images/04-04-websocketserver-span2.png)

We know that the Websocket client calls the server, and you might be wondering, how can we link the calls made from the WebSocket client to the Server? We learnt about ***trace context propagation*** in the previous sections.

In this section, we will explore this concept further in an application that communicates ***asyncronously*** AND using the OneAgent to receive the spans.

In general, a WebSocket client application is designed to fire off a request to the Server and not wait for its response (fire-and-forget). Once the server is ready to respond, it usess the sessionid received from the client to know which client to respond to with what message. This means that the communication between the WebSocket client and the WebSocket server is not syncronous, i.e. not in sequence.

Trace context propgation code should be able to retrieve the spanID of the parent span and then inject it to the current span.

In most cases, the OpenTelemetry SDK and API already takes care of that for us. It is evident when the <mark>**SEND /app/chat/java**</mark> span appear automatically as part of the auto-instrumented PurePaths. This is because the OneAgent recognizes that <mark>**SEND /app/chat/java**</mark> is called by a method which it already has a trace parent ID, and thus follows the spand ID through and creates a "node" in the PurePaths for the OpenTelemetry instrumentation.

However, why are the <mark>**RECEIVE /app/chat/java**</mark> still appearing as a standalone span?

If you would like to see the code for <mark>**RECEIVE /app/chat/java**</mark> spans, open <mark>**WebSocketConfig.java**</mark> from

```
sm-shop/src/main/java/com/salesmanager/shop/store/controller/product/
```

As we are using the OneAgent to handle the OpenTelemetry spans in Java, the OneAgent automatically recognizes the OpenTelemetry API as well. This is evident if you look at the <mark>**code**</mark> tab of the **RECEIVE /app/chat/java** spans.

![websocket span](../../../assets/images/04-04-websocketserver-span-code.png)

In such situations, the OneAgent automatically generates a new trace ID as the OneAgent has no knowledge of the caller. It is such cases where we are using a mix of OneAgent and OpenTelemetry, so we are able to leverage the Dynatrace UI to help in such configurations without the need to write complicated code.

All we need to do now is to tell Dynatrace in **Span context propagation** settings, that if it recognizes a certain SpanKind, attribute, scope etc., propagate the span context so that the traces can be connected. More details can be found in the Dynatrace online documenation [Span context propagation](https://www.dynatrace.com/support/help/shortlink/span-settings#span-context-propagation)  

### 📌 Task

Configure Dynatrace to allow for trace context propagation.

1. Navigate to **Settings > Server-side service monitoring**.
1. Expand it and select  **Span context propagation**.
1. Click on <mark>**Add item** </mark> button.
1. Give a name to the rule.
1. Under the ***Matches*** header, click on <mark>**Add item** </mark> button.

![websocket span](../../../assets/images/04-04-spancontext-config1.png)

Configure the **match** ruleset using the following:
- Source: <mark>**Spand Kind**</mark>
- Value: <mark>**Producer**</mark>

![websocket span](../../../assets/images/04-04-spancontext-config2.png)

💡 **Important:** Remember to click on <mark>**Save changes** </mark>.

The final configuration screen should look like this:

![websocket span](../../../assets/images/04-04-spancontext-configFinal.png)

### Result

Once the configuration is complete, wait a few minutes and choose one of the distributed traces belonging to ***/product/***.

You will see that the SEND span is now connected to the RECEIVE span.

![websocket span](../../../assets/images/04-04-websocketserver-result.png)

### Summary

Configuring trace context propagation can be done via the Dynatrace UI if we are using the OneAgent to receive OpenTelemetry spans.