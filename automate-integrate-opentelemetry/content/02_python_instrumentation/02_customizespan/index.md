## Span customization for process function

Code:
- span.set_attribute("n", n)
- span.add_event("Calculating Fibonacci", {"n": n})

Observe:
- Errors already demo-ed by existing code - explore
- Attributes show up on span. Events also

Understand:
- Very easy to log all sorts of details. See how Dynatrace shows it

Problem: "do  I have to write all this myself?"