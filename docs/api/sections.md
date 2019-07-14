---
layout: doc
title: Sections
subtitle: Learn how to manage sections using the Kirki API
tags: [api]
---

To add a new section using the Kirki API you can use `\Kirki\Section` object:

```php
new \Kirki\Section( 'section_id', array(
    'priority'    => 10,
    'title'       => esc_html__( 'My Section', 'kirki' ),
    'description' => esc_html__( 'My section description', 'kirki' ),
) );
```

If you want to remove a section that has already been added you can use the `remove` method:

```php
$section = new \Kirki\Section( 'section_id' );
$section->remove();
```
