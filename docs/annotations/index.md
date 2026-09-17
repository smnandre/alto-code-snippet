# Custom annotations

An annotation is a byte range plus a type and application-defined metadata.
For example, `new CodeAnnotation(0, 6, 'syntax', ['scope' => 'keyword'])`
can identify `public` at the beginning of a snippet. It does not color the text;
the consumer interprets the type and data.

Attach annotations with `annotate()`. Their offsets are relative to the entire
snippet, while `CodeLine::annotations()` clips them to that line and shifts offsets
to the beginning of the line. Newlines are not part of the line's code.

```text
Snippet "abc\ndef": annotation [2, 5)
  line 1 "abc": [2, 3) covers "c"
  line 2 "def": [0, 1) covers "d"
```

Offsets must be non-negative, lengths positive, and types non-blank. Ranges
must fit inside the snippet. Invalid values raise `InvalidArgumentException`.
Use [literal matching](text.md) when the byte position should follow known text,
and [segments](segments.md) when annotations overlap.
