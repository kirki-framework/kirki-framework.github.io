---
layout: doc
title: option_name
---

If you are saving `options` instead of `theme_mods` and you want to save your options as a serialized array, you can use the `option_name` field to define the `option` in which your options will be saved.

Using this argument requires setting the `option_type` argument to `option`.

Example:

```php
new \Kirki\Field\Text( [
	'settings'    => 'my_setting',
	'label'       => esc_html__( 'Text Control', 'textdomain' ),
	'section'     => 'my_section',
	'default'     => esc_html__( 'This is a default value', 'textdomain' ),
	'option_type' => 'option',
	'option_name' => 'my_option'
] );
```

In the above example the value will be saved as an option and you can retrieve it using something like this:

```php
$option_value = get_option( 'my_option' );
$value        = $option_value['my_setting'];
```

We recommend against using `option` with the customizer unless absolutely necessary.  
If you are building a theme you should use `theme_mod` instead, which is the default for all themes.
