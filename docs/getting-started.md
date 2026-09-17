# Getting started

After [installation](installation.md), save this as `snippet.php` beside `vendor`.
It creates a snippet that starts on line 24 of its original file, then selects its
third line. The package does not read that file; the source name is metadata.

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Code\Snippet\CodeSnippet;

$snippet = CodeSnippet::fromCode(
    "public function run(): void\n{\n    execute();\n}",
    'php',
    sourceName: 'src/Runner.php',
    startLine: 24,
)->selectLines(3);

$line = $snippet->lines()[2];
printf("index=%d number=%d selected=%s\n", $line->index, $line->number, $line->selected ? 'true' : 'false');
echo $line->code, "\n";
```

Run `php snippet.php`. Output:

```text
index=3 number=26 selected=true
    execute();
```

Selection marks a line; it does not remove the others or render a visual effect.
Continue with [selecting and slicing](snippets/selection.md) or
[custom annotations](annotations/index.md).
