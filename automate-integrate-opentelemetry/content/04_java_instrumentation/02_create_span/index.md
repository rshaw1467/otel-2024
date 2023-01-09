## Instrument your code

In this section, we have a Websocket client-server application writen in Java.

The websocket client-server classees have already been instrumented using OpenTelemetry by the developer and the spans are captured by the OneAgent automatically.

Here is an illustration of how the shopizer application communicates with the websocket server.

![websocket](../../../assets/images/04-02-communication.png)

### Viewing the code shopizer application code

First, let us open the shopizer application's code in the <mark>**ShopProductController.java** </mark> class. This class is where the call to the websocket is made. Go to Visual Studio Code and navigate the folder structure below for <mark>**ShopProductController.java** </mark>.

```
sm-shop/src/main/java/com/salesmanager/shop/store/controller/product/
```

Alternatively, you can copy the path below and paste it in the **File > Open File** dialogue box, immediately after <mark>**shopizer** </mark> then click **OK**. the dialouge box.

```
/sm-shop/src/main/java/com/salesmanager/shop/store/controller/product/ShopProductController.java
```

![websocket-send](../../../assets/images/04-02-handlequote2websocket.png)

**Line 118** in the ***handleQuote*** method is the code that initiates the WebSocket client Java object, which in turn will handle sending messages to the WebSocket server.

### Viewing the websocket code instrumented with Open Telemetry

Next, we'll open the websocket code that has been instrumented with Open Telemetry by the websocket application developer. It can be found in the <mark>**TaggingStompSession.java** </mark> class, in the same path as above.

![websocket-instrumentation](../../../assets/images/04-02-TaggingStompSession.png)

**Line 46** to **Line 53** in the ***send*** method is the OpenTelemetry instrumentation code. The code's struture (i.e. declare a tracer, start a span etc.) is similar in nature to the previous section.

There are 2 important differences here and the instructor will highlight them using the slides on the screen. Please turn your attention to the slides.

### Viewing the websocket SEND spans in Dynatrace

As the OneAgent is capturing the spans automatically, we can see the spans as part of a PurePath easily. In the **Dynatrace menu > distributed traces** screen, seach for the the transaction <mark>**product** </mark>.

![distributed traces](../../../assets/images/04-02-traces-product1.png)

Pick any distributed traces in this filtered list. All these traces are generated from the ***ShopProductController*** class. You can see that the websocket SEND span appears between the Python calls.

![distributed traces](../../../assets/images/04-02-traces-product2.png)

However, one question remains. Which method in the ***ShopProductController*** class is making the call to send messages to the websocket server? 

### 📌 Task

**Your Task:** Augment the auto-instrumented ***ShopProductController*** class with OpenTelemetry to "enrich" the distributed traces with more information.

Open up <mark>**ShopProductController.java** </mark> file if you have not already done so.

The tracer has been already pre-setup. All you need to do is to create the spans.

💡 **HINT**
- You can use the code in ***sm-shop/src/main/java/com/salesmanager/shop/store/controller/category/ShoppingCategoryController.java*** as a reference.
- <mark>Line **118** </mark> is the code that you will need to add the OpenTelemetry spans.

As you are making changes to the Java code, you will have to "recompile" the application.

Restart the application to verify any changes:

```bash
Ctrl + C
mvn spring-boot:run
```

### Result

Once you have successfully instrumented the Java code, your distributed traces should look like this.

![Augment java spans](../../../assets/images/04-02-trace2websocket.png)

Notice how Dynatrace handles the parent and child spans nicely without you having the manually code it in OpenTelemetry.

### Summary

You can augment the OneAgent instrumented code using OpenTelemetry. This helps to provide extra context to the distributed traces.

<details>
  <summary>Expand for solution</summary>
  
  ```java
  Span span = getTracer().spanBuilder("java-handleQuote").startSpan();
	try (Scope scope = span.makeCurrent()) {
		MessageClientRunner.getSession().send("/app/chat/java", new ClientMessage(USER_ID, reference));					
	} finally {
		span.end();
  }
  ```
</details>