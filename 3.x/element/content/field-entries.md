---
subtitle: Content Field
---
# Entries

`entries` - links to other entries by UUID or handle.

```yaml
author:
    label: Author
    type: entries
    source: <uuid|handle>
```

The following properties are supported.

Property | Description
------------- | -------------
**source** | the related blueprint UUID or handle name.
**maxItems** | limits the number of entries that can be selected.
**displayMode** | modifies how the field is displayed. Supported values: `relation`, `recordfinder`. Default: `relation`.
**conditions** | specifies a raw where query statement to apply to the model query.
**scope** | applies a [query scope method](../../extend/database/model.md) to the **related form model**, can be a model method name or a static PHP class method (`Class::method`).

To limit the number of selectable items, use the `maxItems` property.

```yaml
author:
    type: entries
    maxItems: 1
```

To display a record finder instead of the typical control, use the `displayMode` property. This mode is only available when one item is selectable.

```yaml
author:
    type: entries
    displayMode: recordfinder
```

## Applying Conditions

You can restrict the related query using SQL or PHP using the approaches below. In the examples, the related record has a field called `is_featured` that renders as a checkbox. We can limit the related records to only those that have this checkbox marked.

### SQL Query Condition

You may limit the related model using a raw SQL query using the `conditions` property.

```yaml
categories:
    label: Categories
    type: entries
    source: Blog\Category
    conditions: is_featured = true
```

### PHP Query Scope

You may limit the related query using a PHP method with the `scope` property.

```yaml
basic_entries:
    label: Basic Entry
    type: entries
    source: Basic\Entry
    scope: App\Classes\ScopeHelper::applyScope
```

This would refer to the `App\Classes\ScopeHelper` class that may look a file located in **app/classes/ScopeHelper.php**, for example.

```php
namespace App\Classes;

class ScopeHelper
{
    public static function applyScope($query)
    {
        return $query->where('is_featured', true);
    }
}
```
