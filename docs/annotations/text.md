# Highlighting text

The released API accepts byte-range annotations. To mark literal text, find its
byte offset with PHP's `strpos()` and attach a `focus` annotation. This complete
example marks every non-overlapping, case-sensitive match:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Code\Snippet\CodeAnnotation;
use Alto\Code\Snippet\CodeSnippet;

$snippet = CodeSnippet::fromCode('run(); run();');
$text = 'run';
$offset = 0;
while (false !== ($offset = strpos($snippet->code(), $text, $offset))) {
    $snippet = $snippet->annotate(new CodeAnnotation($offset, strlen($text), 'focus'));
    $offset += strlen($text);
}
echo json_encode($snippet->annotations(), JSON_THROW_ON_ERROR), "\n";
```

Output:

```text
[{"offset":0,"length":3,"type":"focus","data":[]},{"offset":7,"length":3,"type":"focus","data":[]}]
```

To annotate only the second occurrence, count matches inside the loop and call
`annotate()` only when the count is 2. The loop leaves the snippet unchanged when
no match exists. Reject empty search text before using it; otherwise the loop
cannot advance. This is literal matching, not token or whole-word matching.

`strlen()` and `strpos()` use byte offsets, including for UTF-8. For example,
`strpos('é run', 'run')` is 3, because `é` occupies two bytes.
Use a custom type and data, such as `'warning', ['label' => 'Review this call']`,
when the consumer needs more information than focus alone.
