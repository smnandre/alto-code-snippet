# Public API

Code Snippet exposes immutable source text, line provenance, selections, and
annotations. Use the guides for tasks; consult these contracts for exact behavior.

## `CodeSnippet`

Create a snippet with `CodeSnippet::fromCode()`. Read its code and metadata with `code()`,
`language()`, `sourceName()`, `startLine()`, `endLine()`, and `lineCount()`.

- `selectLines()` marks one-based snippet lines.
- `annotate()` attaches generic byte-range annotations.
- `slice()` projects a half-open byte range and preserves matching metadata.
- `dedent()` and `unindent()` remove common indentation.
- `indent()` adds spaces to non-empty lines.
- `lines()` returns `CodeLine` values.
- `toArray()` and `jsonSerialize()` export the complete model.

Source code and line endings remain byte-for-byte identical until an explicit indentation
transformation is applied.

## `CodeLine`

A line exposes its snippet-relative `index`, original source `number`, plain `code`, selection
state, line-relative annotations, and derived `segments()`.

## `CodeAnnotation`

An annotation contains a byte `offset`, positive `length`, application-defined `type`, and generic
`data`. Its end offset is available through `endOffset()`.

## `CodeSegment`

A segment contains contiguous text and the annotations active across that complete region.

All four value objects implement `JsonSerializable`.

## Coordinates and invalid input

| Input | Contract |
| --- | --- |
| `startLine` and selected lines | Positive one-based numbers; selected lines must exist in the snippet. |
| `slice($startOffset, $endOffset)` | Byte range `[start, end)` inside `code()`, including an empty range. |
| Annotation | Non-negative byte offset, positive length, non-blank type, and a range within the snippet. |
| `indent($spaces)` | Non-negative number of spaces. |
| Language string | A registered Alto Language identifier; `null` leaves the language unspecified. |

Violations raise `InvalidArgumentException`. Byte offsets use `strlen()` and
`substr()` semantics, not Unicode character counts. Original line numbers survive
slicing and indentation changes. Line annotations are clipped to each line;
segments split wherever the set of active annotations changes.

See [Snippets](snippets/index.md) and [Annotations](annotations/index.md) for
complete input/output examples.
