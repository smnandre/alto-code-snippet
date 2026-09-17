# Selecting and slicing

`selectLines()` marks one-based snippet lines and retains the complete code.
`slice()` extracts a half-open byte range: include the start, exclude the end.
Annotations crossing the boundaries are clipped and shifted. Source line numbers
and selections follow the remaining code.

Save and run this standalone script after [installation](../installation.md):

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Code\Snippet\CodeSnippet;

$snippet = CodeSnippet::fromCode("one\ntwo\nthree", startLine: 20)->selectLines(2);
$slice = $snippet->slice(4, 7);
$line = $slice->lines()[0];
printf("%s: index=%d source=%d selected=%s\n", $line->code, $line->index, $line->number, $line->selected ? 'yes' : 'no');
```

Output:

```text
two: index=1 source=21 selected=yes
```

Invalid line numbers and byte ranges raise `InvalidArgumentException`. Use
`lineCount()` or `strlen($snippet->code())` to check the corresponding boundary.
For UTF-8, byte offsets differ from character positions; do not split inside a
multibyte character if the consumer requires valid UTF-8.
