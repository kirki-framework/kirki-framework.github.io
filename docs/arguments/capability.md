---
layout: doc
title: capability
subtitle: Control who can view and edit your fields
---

The `capability` argument allows you to define the the capability that a user must have in order to access the control for a field.

If left undefined it defaults to `edit_theme_options`.

Example:

```php
new \Kirki\Field\Text( [
	'type'        => 'text',
	'settings'    => 'my_setting',
	'label'       => esc_html__( 'Text Control 2', 'textdomain' ),
	'section'     => 'my_section',
	'default'     => esc_html__( 'This is a default value', 'textdomain' ),
	'priority'    => 10,
	// Define a capability.
	'capability'  => 'edit_theme_options'
] );
```

For a complete list of WordPress capabilities and what capabilities each user role has, please visit the [documentation on wordpress.org](https://codex.wordpress.org/Roles_and_Capabilities).
