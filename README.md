# Collector

Named, singleton-per-key collections that gather values under string keys and flatten them out as an array, an HTML string, or JSON — useful for things like collecting per-page `<meta>` tags, breadcrumbs, or validation notices from anywhere in a request.

## Example

```php
use orange\collector\Collector;

$breadcrumbs = Collector::getInstance('breadcrumbs');

$breadcrumbs->add('nav', 'Home')
            ->add('nav', 'Products')
            ->add('nav', 'Widget');

// same collection anywhere else in the request:
Collector::getInstance('breadcrumbs')->add('nav', 'Details');

echo $breadcrumbs->asHtml(' &raquo; ', '<nav>', '</nav>')->collect('nav');
// <nav>Home &raquo; Products &raquo; Widget &raquo; Details</nav>

$breadcrumbs->asJson()->collect('nav');
// ["Home","Products","Widget","Details"]
```

Keys can also be associative, added in bulk:

```php
$meta = Collector::getInstance('meta', ['dedup' => true]);

$meta->add(['description' => 'Some page description', 'og:title' => 'Some Title']);

$meta->collectAll(); // every key => values collected so far
```

Pass `'flat' => true` in the config to store one value per key instead of an array of values, or `'dedup' => true` to silently ignore repeated values within the same key.
