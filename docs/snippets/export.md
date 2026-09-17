# Indentation and export

Use `dedent()` (also named `unindent()`) to remove common indentation, or
`indent($spaces)` to add spaces to non-empty lines. Both preserve source line
numbers and move annotations with the surviving text. Annotations wholly inside
removed indentation disappear; those crossing it are clipped. Blank lines lose
their whitespace. Tabs count as bytes, not visual tab stops.

This complete script shows the transformed code and portable annotation data:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Code\Snippet\CodeAnnotation;
use Alto\Code\Snippet\CodeSnippet;

$snippet = CodeSnippet::fromCode('    run();', sourceName: 'run.php', startLine: 8)
    ->annotate(new CodeAnnotation(4, 3, 'focus'))
    ->dedent();
echo $snippet->code(), "\n";
echo json_encode($snippet->annotations(), JSON_THROW_ON_ERROR), "\n";
printf("source=%s line=%d\n", $snippet->sourceName(), $snippet->startLine());
```

Output:

```text
run();
[{"offset":0,"length":3,"type":"focus","data":[]}]
source=run.php line=8
```

Use `toArray()` or `json_encode($snippet, JSON_THROW_ON_ERROR)` to export the
complete model, including lines and metadata. JSON is portable data; the package
does not provide an HTML renderer or a JSON import factory. Choose escaping and
styles in the consuming application. `indent()` rejects negative spaces.
