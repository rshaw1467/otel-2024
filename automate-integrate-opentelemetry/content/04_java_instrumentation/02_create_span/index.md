## Instrument your code

In this section, we have a Websocket client-server application writen in Java.

The websocket client server code has already been instrumented using Open Telemetry, however they are not showing up in the traces of `ShopProductController`.

Line 118 is the code that sends a message to the websocket server. This code is similar to a "client" making a call to the websocket server.

> **Your Task:** Instrument line 118 so that you can trace the calls made from `ShopProductController` to the websocket server.

Open up ShopProductController.java from in `sm-shop/src/main/java/com/salesmanager/shop/store/controller/product/ShopProductController.java`

line 506 to 508 has already setup the `Tracer` for you. All you need to do is to create the spans.

You can use `sm-shop/src/main/java/com/salesmanager/shop/store/controller/category/ShoppingCategoryController.java` as a reference.

Once you have successfully instrumented the Java code, this is the output you will get in Dynatrace.

![name](../../../assets/images/04-02-xx.png)

<details>
  <summary>Solution here</summary>
  
  ### Heading
  1. Foo
  2. Bar
     * Baz
     * Qux

  ### Some Code
  ```java
  Span span = getTracer().spanBuilder("call-websocket").startSpan();
	try (Scope scope = span.makeCurrent()) {
		MessageClientRunner.getSession().send("/app/chat/java", new ClientMessage(USER_ID, reference));					
	} finally {
		span.end();
   }
   ```
</details>