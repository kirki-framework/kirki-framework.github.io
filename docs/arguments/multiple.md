---
layout: doc
title: multiple
subtitle: Used in select controls to define the maximum number of allowed options
---

The `multiple` argument is only used in the `select` controls.

Its value is an integer and defines the number of simultaneous options the user will be able to choose for the `select` control in question.

Defaults to `1`.

If a value greater than `1` is used then the `<select>` element will allow selecting multiple options, limited by the number specified.

```php
new \Kirki\Field\Select( [
	'settings'    => 'my_setting',
	'label'       => esc_html__( 'This is the label', 'textdomain' ),
	'section'     => 'my_section',
	'default'     => [ 'option-1' ],
	'priority'    => 10,
	'multiple'    => 999,
	'choices'     => [
		'option-1' => esc_html__( 'Option 1', 'textdomain' ),
		'option-2' => esc_html__( 'Option 2', 'textdomain' ),
		'option-3' => esc_html__( 'Option 3', 'textdomain' ),
		'option-4' => esc_html__( 'Option 4', 'textdomain' ),
	],
] );
```

{% include alert.html style="primary" text="CAUTION: When a select control allows selecting multiple elements, it will save an array value instead of a string." %}