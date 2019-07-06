---
layout: doc
title: option_type
---

Allows defining how your options will be saved.
Defaults to `theme_mod`.

Example:

```php
new \Kirki\Field\Text( [
	'settings'    => 'my_setting',
	'label'       => esc_html__( 'Text Control', 'kirki' ),
	'section'     => 'my_section',
	'default'     => esc_html__( 'This is a default value', 'kirki' ),
	'priority'    => 10,
	'option_type' => 'theme_mod',
] );
```

If you are using `option`, you can use `option_name` in combination with this argument to save your options as a serialized array. If `option_name` is not defined, your control will save the value as a separate option in the database.
