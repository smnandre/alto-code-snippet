# Creating snippets

Pass code to `CodeSnippet::fromCode()`. Supply a language slug when known,
`sourceName` for provenance, and `startLine` for the original first line.
The code is stored verbatim, including its line endings. All transformations
return new snippets; retain the returned value.

| Value | Coordinate system | Example |
| --- | --- | --- |
| `CodeLine::index` | One-based line within the snippet | 3 |
| `CodeLine::number` | One-based original source line | 26 when `startLine` is 24 |
| Snippet annotation offset | Bytes within `code()` | Zero starts at the first byte |
| Line annotation offset | Bytes within one line | Reset after clipping to that line |

An empty snippet has zero lines. An unknown explicit language slug or a
non-positive starting line raises `InvalidArgumentException`; omit the language
when it is unknown and supply a positive source position.

For a complete example, see [Getting started](../getting-started.md).
[Code Slicer](https://altophp.com/code-slicer/) reads source and selects declarations.
[Code Highlight](https://altophp.com/code-highlight/) tokenizes code.
[Code Kit](https://altophp.com/code-kit/) connects those stages to this model.
Code Snippet itself neither reads files, parses declarations, nor renders HTML.
