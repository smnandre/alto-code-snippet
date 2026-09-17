# Reading segments

Read each line's `segments()` to get contiguous text regions with a stable set
of active annotations. An overlap splits the text; it does not choose a winning
style. The consumer decides how to combine the active annotations.

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Code\Snippet\CodeAnnotation;
use Alto\Code\Snippet\CodeSnippet;

$snippet = CodeSnippet::fromCode('abcd')->annotate(
    new CodeAnnotation(0, 3, 'focus'),
    new CodeAnnotation(1, 2, 'warning', ['label' => 'Check']),
);
foreach ($snippet->lines()[0]->segments() as $segment) {
    $types = array_map(static fn (CodeAnnotation $a): string => $a->type, $segment->annotations);
    printf("%d %s [%s]\n", $segment->offset, $segment->text, implode(',', $types));
}
```

Output:

```text
0 a [focus]
1 bc [focus,warning]
3 d []
```

Offsets are line-relative. Unannotated text still appears as a segment; an empty
line has none. Escape segment text for the target output format before rendering.
