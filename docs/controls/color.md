---
layout: doc
title: Color Control
subtitle: A flexible Colorpicker control for the WordPress Customizer
tags: [controls]
---

The `kirki-color` control allows you to add a customized color control in the WordPress Customizer.  
The control will automatically detect and use the color-palette defined by your theme as per the [WordPress Docs](https://wordpress.org/gutenberg/handbook/designers-developers/developers/themes/theme-support/), and the defined colors will be used for the control's palette options.  

It is also possible to create a hue-only control.


## Screenshots

A HEX colorpicker:
<p><img src="/uploads/color-hex.png" width="300"></p>

An RGBA colorpicker:
<p><img src="/uploads/color-rgba.png" width="300"></p>

A hue colorpicker:
<p><img src="/uploads/color-hue.png" width="300"></p>

## Adding a Color Field

```php
new \Kirki\Field\Color( [
	'option_type' => 'theme_mod',
	'capability'  => 'edit_theme_options',
	'default'     => '#fff',
	'transport'   => 'refresh', // Or postMessage.
	'label'       => esc_html__( 'My Color Control', 'theme_textdomain' ),
	'section'     => 'colors',
	'choices'     => [
		'alpha' => true,
	]
] );
```

## Field-Specific Options

### Adding Transparency (alpha) to your control

To add support for `rgba` values, you can add this in your control:
```php
'choices' => [
	'alpha' => true,
]
```

### Creating a Hue-Only Control

To make a control hue-only, you can add the following in the array of arguments you use when defining the control:

```php
'mode' => 'hue'
```

This will remove the color-palette, and users will be presented with a simple slider they can use to select their hue.

Please note that when in hue mode, the control will save an `integer` value. To use this in your theme, you may want to use `hsl(a)` colors in your CSS.

## Output

* The `color` field saves a `string` with the `hex` code of the selected color. Example: `#000000`.
* If your control supports transparency and the selected color is not 100% opaque, the value will be saved as a string containing the `rbga` color. Example: `rgba(0,0,0,0)`.
* If your control is a hue-only control, the value saved will be an integer between `0` and `359`.

## Repository

You can find the control's repository on [Github](https://github.com/kirki-framework/control-color/)
