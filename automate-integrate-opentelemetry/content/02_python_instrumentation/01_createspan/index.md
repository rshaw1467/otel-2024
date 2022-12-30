## Create a span for /calc route

Code: with ot.tracer.start_as_current_span("calc"):

Observe:
- Purepath for /calc call now shows in Py service
- Trace automatically extends to process span (from utils.py)
- main.py on port 8080 shows as a separate service with the same URL calls

Understand:
- Local context (same service, everything linked)

Problem: "I need more data on these traces"