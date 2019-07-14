---
layout: doc
title: Panels
subtitle: Learn how to manage panels using the Kirki API
tags: [api]
---

To add a new panel using the Kirki API you can use `\Kirki\Panel` object:

```php
new \Kirki\Panel( 'panel_id', array(
    'priority'    => 10,
    'title'       => esc_html__( 'My Panel', 'kirki' ),
    'description' => esc_html__( 'My panel description', 'kirki' ),
) );
```

If you want to remove a panel that has already been added you can use the `remove` method:

```php
$panel = new \Kirki\Panel( 'panel_id' );
$panel->remove();
```
