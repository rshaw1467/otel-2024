## Propagate Context into OneAgent (Optional)

Code:

```python
with ot.tracer.start_as_current_span("local route"):
    ctx = context.get_current()
    headers = {}
    propagate.inject(headers, ctx)
    return requests.get("http://localhost:8080", headers=headers)
```

Observe:
- local route is start of trace and goes further into OneAgent code
- propagate can be used in either direction, context is the glue to traces